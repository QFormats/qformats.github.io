---
title: Qbsp
layout: default
parent: classes
nav_order: 4
nav_enabled: true
toc_enabled: false
has_toc: false
---

namespace: qformats::qbsp;

```cpp
    int LoadFile(const char* filename)
```    
Loads a BSP level.

returns QBSP_OK if successful.
<br>
```cpp
    uint32_t Version() const
```
Returns the BSP Version of the Level. 
<br>
```cpp
    const SolidEntityPtr WorldSpawn() const
```
Returns the WorldSpawn SolidEntity.
<br>
```cpp
    const map<string, vector<EntityPtr>>& Entities()
```
returns a map that contains vectors to all entites.

Key: Classname
<br>
Val: Vector of EntityPtr

<br>
```cpp
    bool Entities(const string& className, std::function<bool(EntityPtr)> cb) const;
```
```cpp
    const vector<EntityPtr>& PointEntities() const
```
```cpp
    const vector<SolidEntityPtr>& SolidEntities() const
```
```cpp
    static const SolidEntityPtr ToSolidEntity(EntityPtr ent)
```
```cpp
    const bspFileContent& Content() const
```
```cpp
    const vector<bspTexure>& Textures() const
```
```cpp
    const Lightmap* LightMap() const
```
