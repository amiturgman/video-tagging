# VideoTagging Web Element
This web element can be used to annotate videos frame by frame. It is useful when building solutions for video processing  
and there's a need to curate labeled videos for training or testing the computer vision algorithm used.

**General**  

***Regions & Tags***
The element displays a selected video and allows the user to associate tags and text labels per frame.
A **region** is a point or area within the frame, which can then be associated with textual **tags**.

A **region** is represented by a json object, with a structure that depends on the 'type' property.
Examples:  
1) { region: { type: 'point', x: 123, y: 54, radius: 15, tags: [ 'horse', 'brown'] }}  
2) { region: { type: 'rectangle', topLeft{ x: 123, y: 54 }, bottomRight: topLef{ x: 100, y: 10 }, tags: [ 'horse', 'brown'] 
}}

There are 2 region types:  
1) Point - designates an x,y coordinate (can be x or square).  
2) Rectangle - designates a rectangle (x1, y1, x2, y2)  

In addition, it is possible to define single or multiple regions per frame:  
1) Single - only one region can appear in a frame.  
2) Multiple - multiple regions can appear in a frame. 

Once a video has been loaded the control is ready for use:

![Alt text](assets/images/loaded.png?raw=true "Title")


**Video Controls:**

![Alt text](assets/images/videocontrols.png?raw=true "Title")

1) Video timebar.  
2) One frame back  
3) Play/Pause  
4) One frame forward  
5) Go to first untagged frame   
6) Frame number  
7) Play speed  
8) Current and remaining video time  
9) Mute button  
10) Volume slider  

To change the playback speed, click on the icon and select:

![Alt text](assets/images/playback.png?raw=true "Title")


**tagging controls:**

![Alt text](assets/images/taggingcontrols.png?raw=true "Title")
  
1) Tags - toggle the tags to add/remove a tag to/from a region. This is only possible when a region is selected.
   The selected tags are white.  
2) Empty frame - designates a frame as tagged when there are no regions.    
3) Lock tags - automatically adds selected tags to new regions. Toggle to enable/disable. 
     
      

**Usage**

Point/single - On a certain frame, click the video screen. Every click will move the region to a new one.  
Select tags for this region by clicking the tags below.:
![Alt text](assets/images/singlepoint.png?raw=true "Title")

Point/multiple - On a certain frame, click the video screen. Every click adds a new region:
![Alt text](assets/images/multipoints.png?raw=true "Title")

Rectangle single/multiple - Click the video screen and drag. A rectangle appears: 

![Alt text](assets/images/area.png?raw=true "Title")

To select a region, click on it.  
In all modes, when a region is selected, you can add/remove tags to it or delete it altogether.

Lock tags and Auto step - When the Mode is set to Single ("multitags="0"), the video will automatically advance 1 frame 
after a region has been created, so the work flow of a user is:  
     * Create a new region - Click or drag  
     * Select tags  
     * Click on the Lock Icon - turns to white.
     * Create a region  
     * Click the icon again to exit this mode.   

**Technical**

The control is built using HTML5, CSS3 based on the [Polymer](https://www.polymer-project.org/1.0/) 
framework, allowing us to create reusable web components.
Area selection is credited to [ImageAreaSelect](http://odyniec.net/projects/imgareaselect/)

**Installing the control**

bower install CatalystCode/video-tagging


**Hosting the control**   
The control can be hosted in an HTML page. Add these 2 references:

     <link rel="import" href="/bower_components/video-tagging/video-tagging.html">


Add the control label in the place you want the control to appear, wrapped in a div:

    <div style="width:50%">
        <video-tagging id='video-tagging'></video-tagging>
    </div>

A host project can be found in [VideoTaggingTool](https://github.com/CatalystCode/VideoTaggingTool.git) 

**Control API**  
The control recieves and sends data from/to the host.   

Initial Data -   
The following properties must be populated:

   1) videoduration - number, for example 30.07  
   2) videowidth - number, for example 420  
   3) videoheight - number, for example 240  
   4) framerate - number, for example 29.97  
   5) tagshape - string, can be "x" or "square"  
   6) tagtype - string, can be "point" or "square"  
   7) tagsize - number, for example 20 (in pixels)  
   8) multitags - string, can be "0" or "1" 
   9) inputlabelsarray - a string array of the possible labels, for example - ["horse", "bird]
  10) inputFrames - an object containg all the tags of this video (That have been created at an earlier time).
      Example: The object is a dictionary, the frame number is the key. Each frame has a collection of tags.  
      In this example we see data for frames 17, 23 and 41.  
      ![Alt text](assets/images/frames1.png?raw=true "Title")  
      Expanded- each tag is an obect with coordinates and a labels array.  
      ![Alt text](assets/images/frames3.png?raw=true "Title")
  
Call these properties on the control, for example:

    var videolabelging = document.getElementById('video-labelging');
                
        videolabelging.videoduration = data.video.DurationSeconds;
        videolabelging.videowidth = data.video.Width;
        videolabelging.videoheight = data.video.Height;
        videolabelging.framerate = data.video.FramesPerSecond;
        ,,, 
      
  Finally, to load the control, set the src property to the URL of the video: 
 
    videolabelging.src = data.video.Url;

Data from the control -     
When a region is created or updated, the control fires an event called "onregionchanged". Register to this event to get the 
data:

        document.getElementById('video-labelging').addEventListener('onregionchanged', function (e) {...
The control sends **all** the regions and their tags in the current frame. The parameter e holds this data in e.detail:  

![Alt text](assets/images/frames4.png?raw=true "Title")