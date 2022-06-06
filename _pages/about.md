---
layout: default
title: About
permalink: /about/
---

# ImgLib2

ImgLib2 is a general-purpose, multidimensional image and data processing library.

It provides a unified API to work with discrete and continuous n-dimensional data.  This API is interface driven and therefore extensible at will.

ImgLib2 includes implementations of standard numeric and non-numeric data types (8-bit unsigned integer, 32-bit floating point, ...) as well as a number of less typical data types (complex 64-bit floating point, 64-bit ARGB, base pairs, ...).  Data values can be accessed directly or through on-the-fly converters or multi-variate functions.

For discrete data (images, n-dimensional arrays), ImgLib2 implements a variety of memory layouts, data generation, loading, and caching strategies, including data linearized into single primitive arrays, series of arrays, n-dimensional arrays of arrays ("cells"), stored in memory, generated or loaded from disk on demand, and cached in memory or on disk.  Coordinates and values can be accessed directly or through on-the-fly views that invert or permute axes, generate hyperslices or stack slices top higher dimensional datasets, collapse dimensions into vectors

For continuous data (functions, n-dimensional interpolants), ImgLib2 implements a variety of interpolators, geometric transformations, and generator functions.  Coordinates and values can be accessed directly or transformed on-the-fly.

Need a quick start?  Install OpenJDK and maven:
```
sudo apt install openjdk-16-jdk maven
```

Then check out [BigDataViewer vistools](https://github.com/bigdataviewer/bigdataviewer-vistools):
```
git clone https://github.com/bigdataviewer/bigdataviewer-vistools.git
```

Then start JShell in the BigDataViewer vistools project directory:
```
cd bigdataviewer-vistools
mvn compile com.github.johnpoth:jshell-maven-plugin:1.3:run
```

Then try out this code snippet:
```java
import bdv.util.*;
import net.imglib2.position.FunctionRealRandomAccessible;
import net.imglib2.type.numeric.integer.IntType;
import net.imglib2.util.Intervals;

BdvFunctions.show(new FunctionRealRandomAccessible<IntType>(2, (x, y) -> {
  int i = 0;
  double v = 0, c = x.getDoublePosition(0), d = x.getDoublePosition(1);
  for (; i < 64 && v < 4096; ++i) {
    final double e = c * c - d * d;
    d = 2 * c * d;
    c = e + 0.2;
    d += 0.6;
    v = Math.sqrt(c * c + d * d);
    ++i;
  }
  y.set(i);
}, IntType::new), Intervals.createMinMax(-1, -1, 1, 1), "", BdvOptions.options().is2D()).setDisplayRange(0, 64);
```

