[![Join the chat at https://gitter.im/JordanDelcros/OBJImg](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/JordanDelcros/OBJImg?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# OBJIMG

Convert OBJ/MTL files (exported from a 3D soft) into a lightweight image ready for THREE JS (or native WebGL).

## Wait! what?
Ok, an OBJ file contains all informations about the 3D model: vertices, faces, normals, UVs, groups and materials...
All these informations are translated into colours and stored into one single image.

## Why?
First of all, for the fun!

Then cause it save disk space (the compression method can save up to 80% on the file size, or maybe more) and it reduce the files to load from 2 (OBJ and MTL) to only 1 (except textures).

## Example
![sample schema](examples/resources/schema.jpg)

This Lara Croft 3D model contains 74764 vertices, 48549 uvs, 74690 normals and 143290 faces dispatched in 12 differents groups.
On the left, you can see the OBJ/MTL into Blender, at center, the compressed image containing all datas, and on the right, the THREE object builded from the image.

As you can see, the two rendered models looks similar but there is a huge difference, their sizes.
The weight of the OBJ/MTL files is around 14Mo and the compressed image weight is around 4Mo only!

## How to?
The `OBJImg` Class contains both methods to generate and parse the images.

### Generate compressed image
To generate an image model, you can use the `OBJImg` Class script or the node-webkit Application (in progress, OSX only).

#### Using the Class script

It is very easy to implement, just link the `objimg.js` script to your html then do this:
```javascript
OBJImg.generateIMG({
	obj: "path/to/file.obj",
	useWorker: true,
	done: function( datas ){
	
		var image = new Image();
		image.src = datas;
	
	},
	error: function( error ){
	
		console.error(error);
	
	}
});
```
When an image is created, you can access it in the developer tools over resources tab, or you can append it to the DOM to save it or drag it.

##### Options

 - **obj:** the path to the OBJ file or the OBJ content itself
 - **mtl:** the MTL file content (optional, only if the obj is content and not a path)
 - **useWorker:** boolean definning if the script is executed into a webworker to avoid main-thread freezing
 - **done:** callback function with datas parameters (datas is is base64 encoded image)
 - **error:** callback function when the script fail to generate image

If the `obj`parameter is a path, the script will parse the content for a MTL lib (path to the MTL).

#### Using the Command Line Interface

In the terminal, just do `$ npm install -g objimg` then use the `objimg` command to generate PNG image from your model.

```shell
	// generate a png in the same directory with the same name finishing by *.png
	objimg path/to/model.obj 
	
	// generate a png in a special path with defined name
	objimg -o path/to/output.png path/to/model.obj
```

##### options

 - **-o**: choose the output path and file name for the PNG
