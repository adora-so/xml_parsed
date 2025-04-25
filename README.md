`xml_parsed` is a small C++ library that helps serialise XML documents parsed with
[libxml2](https://gitlab.gnome.org/GNOME/libxml2/) into memory chunks and deserialise
memory chunks back into XML documents.

## Usage

To serialise an `xmlDocPtr` (`doc`):

```cpp
#include "xml_parsed.h"

using namespace xml_parsed;

// Map an XML doc to a consecutive chunk of memory:
size_t size; // The size of the returned memory
void *data = xml_doc_wrap(doc, size);

// Handle with data by e.g. writing it to a file
// ...

std::free(data);
```

To deserialise an `xmlDocPtr` from a chunk of memory:

```cpp
#include "xml_parsed.h"

using namespace xml_parsed;

// Read parsed XML doc data some where (e.g. from a file)
void *data = read_from_file("filename");

xmlDocPtr doc = xml_parsed::unwrap(data)

// Handle the returned doc as usual (e.g. evaluate an xpath etc)

std::free(data);
```


## How to build
```
cmake -DCMAKE_BUILD_TYPE=Debug -B build .
cmake --build build
```

Note: on MacOS, you may need to install `pkg-config` by running:
```
brew install pkg-config
```

## CLI

To serialise a HTML file:
```
./build/parsed --serialise files/example.html --output files/example.dat
```

To evaluate an xpath against a html doc:
```
./build/parsed --html files/example.html  -e '//*[last()]' 
```

To evaluate an xpath against a parsed xml doc:
```
./build/parsed --parsed files/example.dat -e '//*[last()]' 
```
