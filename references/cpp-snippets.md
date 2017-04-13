---
layout: page
active: references
title: "C++ Snippets"
---

## Read entire file into string

Note: this only works for ASCII files, Unicode support not guaranteed

```c++
std::ifstream FileHandle(FileName);
std::string String;

FileHandle.seekg(0, std::ios::end);
String.reserve((uint) FileHandle.tellg());
FileHandle.seekg(0, std::ios::beg);

String.assign((std::istreambuf_iterator<char>(FileHandle)), std::istreambuf_iterator<char>());
```

---

## Write an image using stb_image_write

```c++
const int numChannels = 3;
const string fileName = "output.png";
const glm::ivec2 size = glm::ivec2(640, 480);

unsigned char *data = new unsigned char[size.x * size.y * numChannels];

for (int y = 0; y < size.y; ++ y)
{
    for (int x = 0; x < size.x; ++ x)
    {
        unsigned char red, green, blue;

        data[(size.x * numChannels) * y + numChannels * x + 0] = red;
        data[(size.x * numChannels) * y + numChannels * x + 1] = green;
        data[(size.x * numChannels) * y + numChannels * x + 2] = blue;
    }
}

stbi_write_png(fileName.c_str(), size.x, size.y, numChannels, data, size.x * numChannels);
delete[] data;

```
