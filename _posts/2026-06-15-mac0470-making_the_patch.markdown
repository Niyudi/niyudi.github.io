---
layout: post
title:  "MAC0470 - Making the patch"
date:   2026-06-15
categories: mac0470
---

Recently, in rust-next, a new crate was approved and the mantainers signaled a new good first issue related to it.

In the kernel, there's a module named *transmute*, which contains a trait *FromBytes*. This is intended to be implemnted for types that can be interpreted from any bit pattern. This gives flexibility to the type, and avoids mistakes in this kind of conversion that requires unsafe code.

Recently, the crate *zerocopy* was approved for use in the kernel, which implements *FromBytes* itself. Thus, the task at hand is to replace uses of *transmute::FromBytes* with *zerocopy::FromBytes*. It is a pretty manual task, but it should be quite simple.

Thus, in search for uses of the trait I found the *nova_core* has some uses that are simple to replace. For example, in *firmware.rs*, a few structs are defined that implement the trait, and one of them already uses the new derive. So it should be similarly easy to change the others to use the new one.

For example, the struct *FalconUCodeDescV3* has the implementation:

{% highlight rust %}
unsafe impl FromBytes for FalconUCodeDescV3 {}
{% endhighlight %}

*zerocopy*'s version of *FromBytes* can only be implemented by dervie, for safety reasons. Since it is already in *kernel::prelide::\**, all we have to do is derive it and remove this implementation:

{% highlight rust %}
#[repr(C)]
- #[derive(Debug, Clone)]
+ #[derive(Debug, Clone, FromBytes)]
pub(crate) struct FalconUCodeDescV3 {
    ...
}

...

- unsafe impl FromBytes for FalconUCodeDescV3 {}
{% endhighlight %}

This struct is is used in *vbios.rs* as:

{% highlight rust %}
let v3 = FalconUCodeDescV3::from_bytes_copy_prefix(data)
    .ok_or(EINVAL)?
    .0;
{% endhighlight %}

This function does not exist as is in *zerocopy*, but there's an equivalent *read_from_prefix*. The change is then:

{% highlight rust %}
let v3 = FalconUCodeDescV3::read_from_prefix(data)
    .map_err(|_| EINVAL)?
    .0;
{% endhighlight %}

This can be done similarly with other structs in the file.