# How to serve website from subdirectory of other website

Today I had to migrate one website within other which is easy task, but ...

## Use case


Let's say we have 2 websites on 2 different domains, we need one site to accessible on subtoute of the other. This is easy as 1 - 2 -3.

We have domain1.com and domain2.com
we want to domain1.com/othersite to serve domain2.com content.

```
domain1.com/othersite -> serve content of domain2.com 
```

1. We need to **change base path** of all paths on website domain2 to the root of _domain1.com/othersite_
    ```
    # examples
    domain2.com/about.html      -> domain1.com/othersite/about.html
    domain2.com/images/face.png -> domain1.com/othersite/images/face.png
    ```

1. Now we need to copy all files of domain2.com to the root of the domain1.com under the othersite subfolder.
1. Now test all links and images if they work correctly.
