# osm4scala

[![Build Status](https://travis-ci.org/angelcervera/osm4scala.svg)](https://travis-ci.org/angelcervera/osm4scala)
[![Coverage Status](https://coveralls.io/repos/github/angelcervera/osm4scala/badge.svg?branch=master)](https://coveralls.io/github/angelcervera/osm4scala?branch=master)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/angelcervera/osm4scala/master/LICENSE.md)

Scala library focus in parse PBF2 Open Street Map files as iterators.

At the moment, practically all Open Street Map data distribution are published using the osm pbf format because for publishing/distribution it is looking for size save. 
But because this format has been designed to achieve really good compression, it is really complex to obtain an optimized way to process its content with moderns big data tools.

## Goal
With this library, you can forget about complexity of the osm.obf format and think about a **scala iterators of primitives** (nodes, ways and relations) or blob blocks.

For example, count all node primitives in a file is so simple as:
```scala
EntityIterator.fromPbf(inputStream).count(_.osmModel == OSMTypes.Node)
```

## Performance
The performance of the first version looks really good.

For example, it expends only **32 seconds to iterate over near of 70 millions** of elements that compose Spain. 
Below the result of few executions of the [Primitives Counter Example](blob/master/examples/counter/src/main/scala/com/acervera/osm4scala/examples/counter/Counter.scala) available in the code.
~~~~
Found [67,976,861] primitives in /home/angelcervera/projects/osm/spain-latest.osm.pbf in 32.44 sec.
Found [4,839,505] primitives of type [Way] in /home/angelcervera/projects/osm/spain-latest.osm.pbf in 31.72 sec.
Found [63,006,432] primitives of type [Node] in /home/angelcervera/projects/osm/spain-latest.osm.pbf in 32.70 sec.
Found [130,924] primitives of type [Relation] in /home/angelcervera/projects/osm/spain-latest.osm.pbf in 32.66 sec.
~~~~

The other example, [Tag Extraction Example](blob/master/examples/tagsextraction/src/main/scala/com/acervera/osm4scala/examples/tagsextraction/TagExtraction.scala)
expends only 42 seconds to extract the list of all unique tags from the Spain pbf.
~~~~
Found [4,166] different tags in /home/angelcervera/projects/osm/spain-latest.osm.pbf. List stored in /home/angelcervera/projects/osm/spain-latest.tags.txt. Time to process: 39.22 sec.
Found [2,451] different tags in primitives of type [Way] in /home/angelcervera/projects/osm/spain-latest.osm.pbf. List stored in /home/angelcervera/projects/osm/spain-latest.tags.txt. Time to process: 33.47 sec.
~~~~

And the use of memory is negligible.

It is necessary take into account that this first version is a **single thread** implementation. In the next version, I will work into a paralellization to boost the speed processing.

Specs of the computer (laptop) used to execute the test:
```
Ubuntu 16.04.1 LTS Desktop / 64bits
Intel(R) Core(TM) i7-4712HQ CPU @ 2.30GHz
2x8GiB SODIMM DDR3 Synchronous 1600 MHz (0.6 ns)
512GB SAMSUNG SSD
```

##  Examples:
In the project, there is a folder called "examples" with few simple examples.

### Counter.

Count the number of primitives in a pbf file, with he possibility of filter by primitive type.

### Tags extraction.

Extract a list of unique tags from a pbf file, with he possibility of filter by primitive type.
The list is stored in a file given as parameter.


## Requirements:

- Protobuf compiler (only if you want build the library):
    
    Every OS has a different installation process. Has been tested with version 2.6.1
    
    Install in Ubuntu:
    ```
    sudo apt-get install protobuf-compiler
    ```
- Import the library using maven or sbt.
    ```
    libraryDependencies += com.acervera.osm4scala %% osm4scala % 1.0
    ```

## As reference:

  - PBF2 Documentation: http://wiki.openstreetmap.org/wiki/PBF_Format
  - PBF2 Java library: https://github.com/openstreetmap/osmosis/tree/master/osmosis-osm-binary
  - Download whole planet pbf files: http://free.nchc.org.tw/osm.planet/
  - Download country pbf files: http://download.geofabrik.de/index.html
  - Scala protocol buffer library: https://scalapb.github.io/

## Libraries:

  - ScalaPB: https://scalapb.github.io/
  
