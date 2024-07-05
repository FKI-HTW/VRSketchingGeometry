# VR Sketching Geometry
Work in progress. Features may not be complete and the API may change.

This is a framework for developing 3D sketching applications in Unity.

## Features:
- smoothly interpolated lines
- patch surfaces
- ribbon shaped lines
- organisation of sketch objects with groups
- undo and redo
- serialization of sketches
- OBJ export of sketches

## Installation

### Manually from a local folder
- open the Package Manager
- click on "+"
- choose "Add package from disk..."
- locate the downloaded package
- select the package.json file within the package

### Automatically from URL
- open the Package Manager
- click on "+"
- choose "Add package from git URL..."
- enter the URL 
``` 
https://github.com/hunsri/VRSketchingGeometryUP.git#0.1.0
```

## Import the examples
- open the Package Manager
- locate the installed package
- click on "Samples"
- click on "Import"
- files will be imported under `Assets/Samples/`

## API documentation
Read the **developer guide** and **API documentation** at the GitHub pages site. </br>
Alternatively, you can host these yourself too,
check out the [Docs Reference](../../Docs.md) for further details!

## Quick start
The following example script shows how to create a new line sketch object and add few control points to it using a command invoker.</br>
At the end one command is undone.</br></br>
You will have to reference a `DefaultReference` Scriptable Object in the inspector.</br>
An example can be found under `SharedAssets/Assets/DefaultReferences.asset`.</br>
To access `SharedAssets` you have to import it in the Package Manager (see [Import the examples](./README.md#import-the-examples))</br>

```C#
    using UnityEngine;
    using VRSketchingGeometry.SketchObjectManagement;
    using VRSketchingGeometry;
    using VRSketchingGeometry.Commands;
    using VRSketchingGeometry.Commands.Line;

    public class CreateLineSketchObject : MonoBehaviour
    {
        public DefaultReferences Defaults;
        private LineSketchObject LineSketchObject;
        private SketchWorld SketchWorld;
        private CommandInvoker Invoker;

        void Start()
        {
            SketchWorld = Instantiate(Defaults.SketchWorldPrefab).GetComponent<SketchWorld>();
            LineSketchObject = Instantiate(Defaults.LineSketchObjectPrefab).GetComponent<LineSketchObject>();
            Invoker = new CommandInvoker();
            Invoker.ExecuteCommand(new AddObjectToSketchWorldRootCommand(LineSketchObject, SketchWorld));
            Invoker.ExecuteCommand(new AddControlPointCommand(this.LineSketchObject, new Vector3(1, 2, 3)));
            Invoker.ExecuteCommand(new AddControlPointCommand(this.LineSketchObject, new Vector3(1, 4, 2)));
            Invoker.ExecuteCommand(new AddControlPointCommand(this.LineSketchObject, new Vector3(1, 5, 3)));
            Invoker.ExecuteCommand(new AddControlPointCommand(this.LineSketchObject, new Vector3(1, 5, 2)));
            Invoker.Undo();
        }
    }
```

## Workflow
1. Instantiate a sketch world prefab. Easy access to prefabs is provided through the DefaultReferences asset at `SharedAssets/Assets/DefaultReferences.asset`.
2. Create sketch objects and groups from prefabs and add them to the sketch object world. Execute commands using a CommandInvoker object for undo and redo functionality. All scripts are in the VRSketchingGeometry namespace.
4. Serialize or export using methods of the sketch world script.
5. Load serialized sketch world from the serialized xml file for further editing.

The [LegacyExample](./README.md#legacy-example) also shows this process in practice.

## Samples

Various examples are provided under `VRSketchingGeometryPackage/Samples`.
These can be imported via the Unity Package Manager.
(see [Import the examples](./README.md#import-the-examples))

### Shared Assets
Contains all the assets required to run the examples.
Can be used as a reference for the creation of own assets.

### [Example Scenes](./Samples/ExampleScenes/Scenes/README.md)
Contains various examples. For further details please refer to [this readme](./Samples/ExampleScenes/Scenes/README.md).

### Legacy Example
Contains a static scene with various messy test scripts and corresponding game objects.
![sampleScene](https://user-images.githubusercontent.com/51961152/192534926-2c6406b1-4556-4808-baf7-9f8eeab0bc5f.png)

## Notes
This is a conversion from the plugin version (https://github.com/tterpi/VRSketchingGeometry) to a package version.
Originally based on code from: https://github.com/bittermanq/KochanekBartelsSplines

## Tests

There are unit tests using Unity Testing Framework. (https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/index.html)
They mostly cover the undoable commands and the generation and applying of data objects.
Coverage of the unit tests should be expanded.

**Before you can run the tests, you have to make sure the `CommandTestScene` is added in the Build Settings**</br>
The tests are located at `Assets/VRSketchingGeometryPackage/Tests`.</br>
To do that go to "File>Build Settings..." and check if `VRSKetchingGeometryPackage/Tests/Scenes/CommandTestScene` is present there.</br>
If that's not the case you have to add it.</br>

![BuildSettings](https://user-images.githubusercontent.com/51961152/195391439-bf552078-04a4-4722-aa1f-a12d7c8d21d0.png)


**To run the tests you have to open the Test Runner**</br>
You can find it under "Window>General>Test Runner".</br>
Once the Test Runner is open, click on "Play Mode".</br>
You can then select and perform the tests of your choice!</br>

![TestRunner](https://user-images.githubusercontent.com/51961152/195391886-76177d36-d95f-46c4-beba-3a93c37cb2f8.png)
![UnitTestSelection](https://user-images.githubusercontent.com/51961152/195391910-92307dfc-6cba-44df-b87e-50a8dfba7976.png)
