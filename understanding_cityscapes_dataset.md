Datasets for cityscapes can be downloaded [here](https://www.cityscapes-dataset.com/downloads/). (We have to create an account). There are two main files: leftImg8bit_trainvaltest.zip and gtFine_trainvaltest.zip. 

# leftImg8bit_trainvaltest.zip
True images are stored in leftImg8bit_trainvaltest.zip. In this folder, we can see real (reality/true/ground truth) pictures of roads, car driving, etc.
- The folder leftImg8bit_trainvaltest.zip when unzipped, will have sub-folders with names {aachen, frankfurt, etc}. Here is an example of the file 'aachen_000000_000019_leftImg8bit.png'
  
 <img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_leftImg8bit.png" width="70%">
  
- Each picture has its 'segmentation' verions which are stored in gtFine_trainvaltest.zip: _gtFine_color.png, _gtFine_instanceIds.png, _gtFine_labelIds.png, _gtFine_polygons.json

<img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_color.png" width="40%">   <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_instanceIds.png' width ='40%'>
  
  !<img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_labelIds.png' width = '40%'>
  
  
 - The true image is segmented (partitioned) into regions. Each region belongs to [34](https://github.com/mcordts/cityscapesScripts/issues/8) [classes](https://www.cityscapes-dataset.com/dataset-overview/#labeling-policy), [says](https://github.com/mcordts/cityscapesScripts/blob/master/cityscapesscripts/helpers/labels.py), car, [truck](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L91), [tree](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L85), [pole](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L81), [person](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L88), etc. Files in gtFine give 4 different ways to encode this partion.
 
# gtFine_trainvaltest.zip

- gtFine_color.png: <img src="https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_color.png" width="10%"> . This file is of type RGB. Each pixel is colored according to its classes. There is no distinstion between car1 or car2, etc. (RGB for [car](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L90) is (  0,  0, 142)) 

- gtFine_labelIds.png:  <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_labelIds.png' width = '20%'>a greyscale image ([mode](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes) = 'L'). Each pixel has value (multiplied by 255) in [0,1,...,33,255]. Meaning of these numbers can be seen [here](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py#L63) This file is equivalent to gt_Fine_color.png (just a different way of coding pixels).

- gtFine_instanceIds.png: <img src='https://raw.githubusercontent.com/tranvohuy/Enet-cityscapes/master/readme_files/aachen_000000_000019_gtFine_instanceIds.png' width ='20%'>  a greyscale image ([mode](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes) = 'I'). This image only cares about _important_ objects (instances) like cars, persons, bike, etc. It ignores building, wall, trees, etc.



  


- gtFine_polygons.json: a json-type file. It stores information for each region (partition/ segmentation) of the true image. Each region has ```label``` and ```polygon```.  An example in aachen_000000_000019_gtFine_polygons.json
  ```
  {   "label": "pole", 
            "polygon": [
                [ 102, 278], 
                [ 103, 243], 
                [ 112, 243], 
                [ 111, 281]
            ]
    }, ```
      
This means, there is a 'pole' object. It is a polygon from pixels with coordinate (102,278)-(103,243)-(112,243)-(111,281). The image dimension is 1024x2048.
      
The json files seem to have the most information about segmentation. We can see from the files that there are many object poles, 11 object cars, 2 object people, etc. One true car can be splitted into several object cars because of poles or trees in front of them.
 
 # Convert file types
 
 There is a way to convert json file to three other image file types using [json to instanceid](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/json2instanceImg.py) [images](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdInstanceImgs.py), [json to label](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/json2labelImg.py) [images](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py). This is useful since most of us don't have a strong computing resource to run DL. We can reduce the number of object classes from 34 to, says, 10 by combining trees, building, poles as one class.  With a smaller number of classes, it's quicker and easier to run DL (as least for curiosity)
 
 ## [CreateTrainIdLabelImgs.py](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py)
Suppose we want to recognize 10 classes.
- Go to [```labels.py```](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/helpers/labels.py), change ```trainId``` accordingly.
- Go to [```CreateTrainLabelImgs.py```](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py), change directory and files from line [32](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py#L32) to [50](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py#L50) accordingly. For example,
```
    cityscapesPath = '.' # i.e. the folder containing the gtFine folder
    # how to search for all ground truth
    searchFine   = os.path.join( cityscapesPath , "gtFine"   , "*" , "*" , "*_gt*_polygons.json" )

    # search files
    filesFine = glob.glob( searchFine )
    filesFine.sort()
    
    files = filesFine

    # quit if we did not find anything
    if not files:
        printError( "Did not find any files. Please consult the README." )
```
   
  - From line [61](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/createTrainIdLabelImgs.py#L61), for each file with ending ```_polygons.json```, it will run [```json2labelImg(f, dst, "trainIds")```](https://github.com/mcordts/cityscapesScripts/blob/4b6e5154281617660d8347e6c7109686af239317/cityscapesscripts/preparation/json2labelImg.py#L133)
``` 
def json2labelImg(inJson,outImg,encoding="ids"):
    annotation = Annotation()
    annotation.fromJsonFile(inJson)
    labelImg   = createLabelImage( annotation , encoding )
    labelImg.save( outImg )
```
