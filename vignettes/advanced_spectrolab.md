# Advanced spectrolab
Jose Eduardo Meireles  


```r
library("spectrolab")
```

## The spectra class

The `spectra` class holds the essential information used in spectral dataset: reflectance, wavelengths, etc. The class has a bunch of requirements in terms of both format and values. 

Some of the requirements can be relaxed. For example, reflectance must by default be between 0 and 1, but you can pass `enforce01` to the `spectra()` constructor to supress this behaviour

## Constructing a `spectra` object _"by hand"_

Alternativelly to `read_spectra()` or `as.spectra()`, you can create a `spectra` object "by hand"" using the more flexible `spectra()` constructor, which takes at least arguments: (1) a reflectance matrix, (2) a vector of wavelengths and (3) the sample names.


```r
# (1) Create a reflectance matrix.
#     In this case, by removing the first column that holds the species name
rf = spec_matrix_example[, -1]

# (2) Create a vector with wavelength labels that match
#     the reflectance matrix columns.
wl = colnames(rf)

# (3) Create a vector with sample labels that match
#     the reflectance matrix rows.
#     In this case, use the first colum of spec_matrix_example
sn = spec_matrix_example[, 1] 

# Finally, construct the spectra object using the `spectra` constructor
spec = spectra(reflectance = rf, wavelengths = wl, names = sn)

# And hopefully this worked fine
is_spectra(spec)
```

```
## [1] TRUE
```

```r
plot(spec)
```

![](advanced_spectrolab_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

## Getting and Setting

`spectrolab` gives you acess to get and set functions for most `spectra` components. The `names()`, `wavelengths()` functions do both getting and setting, as expected in T


```r
# Getters
names(spec)[1:4]
wavelengths(spec)[1:4]

# Setters
names(spec)       = toupper(names(spec))
```

Reflectances are set using the `[` notation. For instance:


```r
spec[1, 400:1200] = spec[1, 400:1200] * 2
plot(spec)
```

![](advanced_spectrolab_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

You may want to fiddle with the reflectance itself. This is easy to do, but there are some constraints. For example, `spectrolab` will not allow you to have negative reflectance values or values greater than 1.

However, `spectrolab` will throw an error if you try to perform an illegal operation to reflectance, for instance 


```r
# Trying to add 1.0 to all reflectance values will fail.
spec[] = reflectance(spec) + 1.0
```