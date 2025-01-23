---
title: usage
layout: default
parent: libqbsp
nav_order: 2
toc_enabled: false
---

### Usage

Usage depends heavily on what you're using and what you want to do, i'll give you some simple examples of some common or essential use-cases.

Vertices are converted to the OpenGL coordinate system.

<br>
#### Load a BSP file
<br>
Loading a BSP file is pretty straight forward. Just create a new Object of type "qformats::qbsp::QBsp" and call the LoadFile method.

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

<br>
<br>

#### Show all Entites and Iterate over a specific Entity class
<br>
A Quake Level usually has a bunch of entites like info_player_start, worldspawn and light. libqbsp provides some lists to easily fetch them and their properties.

We have to deal with two different types of Entites:

* PointEntity -> an Entity that has an origin and sometimes an angle, but no (level) geometry.

* SolidEntity -> an Entity that has geometry defined in the level. a trigger volume, door or simply a detached chunk of the level. The geometry is usually defined in worldspace, so we don't need an origin.

libqbsp provides two classes to make this data accessible:

* BaseEntity -> is a PointEntity
* SolidEntity -> is a SolidEntity, it derives from BaseEntity.

<br>

Lets Iterate over all Entites that have Geometry attached to them (SolidEntities) and print their classname:

```cpp
#include <qbsp/qbsp.h>
#include <iostream>

using std::cout, std::endl;

int main(int argc, char *argv[])
{
    auto bsp = new qformats::qbsp::QBsp();
    auto res = bsp->LoadFile("E1M1.bsp");

    for (const auto &se : bsp->SolidEntities())
    {
        cout << se->Classname() << endl;
    }

    return 0;
}
```
<br>
<br>

#### Show all Entites and Iterate over a specific Entity class
<br>
A Quake Level usually has a bunch of entites like info_player_start, worldspawn and light. libqbsp provides some lists to easily fetch them and their properties.


Lets Iterate over all Entites that have Geometry attached to them (SolidEntities) and print their classname:

```cpp
#include <qbsp/qbsp.h>
#include <iostream>

using std::cout, std::endl;

int main(int argc, char *argv[])
{
    auto bsp = new qformats::qbsp::QBsp();
    auto res = bsp->LoadFile("E1M1.bsp");

    for (const auto &se : bsp->SolidEntities())
    {
        cout << se->Classname() << endl;
    }

    return 0;
}
```
trigger_setskill

Okay fine, now we want to find out how many lights we have in our level. We can ask our qbsp object to give us a vector of all entites of class "light"

```cpp
#include <qbsp/qbsp.h>
#include <iostream>

int main(int argc, char *argv[])
{
    auto bsp = new qformats::qbsp::QBsp();
    auto res = bsp->LoadFile("E1M1.bsp");

    const auto &lightsInLevel = bsp->Entities().find("light2");

    // check if we found the class in our map.
    // if yes -> print the size of our entity vector
    if (lightsInLevel != bsp->Entities().end())
    {
        cout << lightsInLevel->second.size() << endl;
    }

    return 0;
}
```
<br>
<br>

#### working with entity data
<br>

Our next goal is to gather some informations from our Entities.
Let's first list all attributes of the "trigger_setskill" entity class.

```cpp
#include <qbsp/qbsp.h>
#include <iostream>

using std::cout, std::endl;

int main(int argc, char *argv[])
{
    auto bsp = new qformats::qbsp::QBsp();
    auto res = bsp->LoadFile("E1M1.bsp");

    bsp->Entities("trigger_setskill", [](const qbsp::EntityPtr skillTrigger) -> bool {
        if (skillTrigger->Type() == qbsp::ETypeSolidEntity)
        {
            cout << "is SolidEntity" << endl;
        }
        for (const auto &attrib : skillTrigger->Attributes())
        {
            cout << "attrib name: " << attrib.first << endl;
        }
        return true;
    });

    return 0;
}
```

Okay, we now know a couple of things. First we know that trigger_setskill is a SolidEntity, and second, we know that we have an attribute named "message". Let's print the value of this attribute.

```cpp
#include <qbsp/qbsp.h>
#include <iostream>

using std::cout, std::endl;

int main(int argc, char *argv[])
{
    auto bsp = new qformats::qbsp::QBsp();
    auto res = bsp->LoadFile("E1M1.bsp");

    bsp->Entities("trigger_setskill", [](const qbsp::EntityPtr skillTrigger) -> bool {
        // convert our shared Entity pointer to a SolidEntity pointer.
        auto skillTrigger = qbsp::QBsp::ToSolidEntity(ent);
        cout << skillTrigger->Attributes().find("message")->second << endl;
        return true;
    });

    return 0;
}
```