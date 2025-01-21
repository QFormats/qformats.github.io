---
title: usage
layout: default
parent: libqbsp
toc_enabled: false
---

### Usage

here is some basic code that will load a BSP file:

```cpp

#include <qbsp/qbsp.h>
#include <iostream>

int main() {
    auto bsp = new qformats::qbsp::QBsp(qformats::qbsp::QBspConfig{.loadTextures = true});
    auto res = bsp->LoadFile("E1M1.bsp");

    if (res != qformats::qbsp::QBSP_OK)
    {
        cout << "BSP ERROR: " << res << endl;
        return -1;
    }

    return 0;
}
```