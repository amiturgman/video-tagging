# VideoTaggingControl
A control for video tagging

**General**

The control displays a selected video and allows the user to tag frames by clicking on the video screen.
Once clicked, a **location** is created on the video screen.
The user can select any location and add **tags** to it.

There are 2 location types:
1) Point - designates an x,y coordinate.
2) Area - designates a rectangle (x1, y1, x2, y2)

In addition, it is possible to define single or multiple tags per frame:
1) Single - only one location can appear.
2) Multiple - multiple location can be designated.

Once a location is created, it is selectable by clicking on it.
When a location is selected, the user can tag it by clicking on the tag buttons.

The control is loaded initially with no video:

![Alt text](images/empty.png?raw=true "Title")

Once a video has been loaded the control is ready for use:

![Alt text](images/loaded.png?raw=true "Title")

**Video Controls:**

![Alt text](images/videocontrols.png?raw=true "Title")

1) Video timebar.
2) One frame back
3) Play/Pause
4) One frame forward
5) Frame number
6) Playback control
7) current and remaining video time
8) Mute button
9) Volume slider

To change the playback rate, click on the icon and select:

![Alt text](images/playback.png?raw=true "Title")


**Tagging controls:**

![Alt text](images/taggingcontrols.png?raw=true "Title")

1) Next untagged - jumps to the first frame that conatins no locations/tags.  
2) Tags - toggle the tags to add/remove a tag to/from a location. This is only possible when a location is selected.
   The selected tags are white.  
3) Empty frame - designates a frame as tagged when there are no locations.    
4) Lock Tags - automatically adds selected tags to new locations. Toggle to enable/disable.

**Usage**

Point/single - On a certain frame, click the video screen. Every click will move the location.
![Alt text](images/singlepoint.png?raw=true "Title")

Point/multiple - On a certain frame, click the video screen. Every click adds a new location
![Alt text](images/multipoints.png?raw=true "Title")

Area - Click the video screen and drag. A rectangle appears. You can move it for repositioning. When done, click the video screen:
![Alt text](images/area.png?raw=true "Title")

![Alt text](images/area2shapes.png?raw=true "Title")

In all modes, when a location is selected, you can add/remove tags to it or delete it.

**Technical**

The control is built using HTML, CSS and the <a href="https://www.polymer-project.org/1.0/" target="_blank">Polymer</a>
framework, allowing us to create reusable web components.


**Installing the control**

run:
bower install

demo - public

**Hosting the control**   
The control can be hosted in an HTML page. Add these 2 references:

     <script src="/bower_components/webcomponentsjs/webcomponents.js"></script>
     <link rel="import" href="video-tagging.html">


Add the control tag in the place you want the control to appear, wrapped in a div:

    <div style="width:800px;height:600px;margin:50px 0px 0px 200px;">
        <video-tagging id='video-tagging'></video-tagging>
    </div>

**Control API**
The control recieves and sends data from/to the host.   

Initial Data -   
The following properties must be populated:

   1) videoduration - number, for example 30.07  
   2) videowidth - number, for example 420  
   3) videoheight - number, for example 240  
   4) framerate - number, for example 29.97  
   5) locationshape - string, can be "x", "rectangle" or "circle"  
   6) locationtype - string, can be "point" or "area"  
   7) locationsize - number, for example 20 (in pixels)  
   8) multilocations - string, can be "0" or "1" 
   9) inputtagsarray - a string array of the possible tags, for example - ["horse", "bird]
  10) inputFrames - an object containg all the locations of this video (That have been created at an earlier time).
      Example: The object is a dictionary, the frame number is the key. Each frame has a collection of locations.  
      In this example we see data for frames 17, 23 and 41.  
      ![Alt text](images/frames1.png?raw=true "Title")  
      Expanded- each location is an obect with coordinates and a tags array.  
      ![Alt text](images/frames3.png?raw=true "Title")
  Call these properties on the control, for example:

    var videotagging = document.getElementById('video-tagging');
                
        videotagging.videoduration = data.video.DurationSeconds;
        videotagging.videowidth = data.video.Width;
        videotagging.videoheight = data.video.Height;
        videotagging.framerate = data.video.FramesPerSecond;
        ,,, 
      
  Finally, to init the control, call the initVideo function, passing in the video URL as an argument: 
 
    videotagging.initVideo(data.video.Url);

Tagging Data -     
When a location is created or updated, the control fires an event called "onlocationchanged". Register to this event to get the data:

    window.addEventListener('onlocationchanged', function (e) {...
The control sends **all** the locations and their tags in the current frame. The parameter e holds this data in e.detail.frame:  

![Alt text](images/frames4.png?raw=true "Title")

