# VideolabelgingControl
A control for video tagging

**General**

The control displays a selected video and allows the user to tag frames by clicking on the video screen.
Once clicked, a **position** is created on the video screen.
The user can select any position and add **labels** to it.

There are 2 position types:  
1) Point - designates an x,y coordinate.  
2) Area - designates a rectangle (x1, y1, x2, y2)  

In addition, it is possible to define single or multiple positions per frame:  
1) Single - only one position can appear in a frame.  
2) Multiple - multiple positions can appear in a frame. 

Once a video has been loaded the control is ready for use:

![Alt text](images/loaded.png?raw=true "Title")


**Video Controls:**

![Alt text](images/videocontrols.png?raw=true "Title")

1) Video timebar.  
2) One frame back  
3) Play/Pause  
4) One frame forward  
5) Frame number  
6) Playback speed control  
7) current and remaining video time  
8) Mute button  
9) Volume slider  

To change the playback speed, click on the icon and select:

![Alt text](images/playback.png?raw=true "Title")


**tagging controls:**

![Alt text](images/labelgingcontrols.png?raw=true "Title")

1) Next untagged - jumps to the first frame that does not contain a position.  
2) labels - toggle the labels to add/remove a label to/from a position. This is only possible when a position is selected.
   The selected labels are white.  
3) Empty frame - designates a frame as tagged when there are no positions.    
4) Lock labels - automatically adds selected labels to new positions. Toggle to enable/disable. 
     
      

**Usage**

Point/single - On a certain frame, click the video screen. Every click will move the position to a new one:
![Alt text](images/singlepoint.png?raw=true "Title")

Point/multiple - On a certain frame, click the video screen. Every click adds a new position:
![Alt text](images/multipoints.png?raw=true "Title")

Area - Click the video screen and drag. A rectangle appears: 

![Alt text](images/area2shapes.png?raw=true "Title")

In all modes, when a position is selected, you can add/remove labels to it or delete it.

Lock labels and Auto step - When the Mode is set to "Single", the video will automatically advance 1 frame after a position has been designated, so the work flow of a user is:  
     Create a new position - Click or drag  
     Select labels  
     Click on the Lock Icon - turns to white.
     Click the icon again to exit this mode.   

**Technical**

The control is built using HTML5, CSS3 based on the <a href="https://www.polymer-project.org/1.0/" target="_blank">Polymer</a>
framework, allowing us to create reusable web components.
Area selection is credited to <a href="http://odyniec.net/projects/imgareaselect/" target="_blank">ImageAreaSelect</a>

**Installing the control**

run:
bower install CatalystCode/video-tagging


**Hosting the control**   
The control can be hosted in an HTML page. Add these 2 references:

     <script src="/bower_components/webcomponentsjs/webcomponents.js"></script>
     <link rel="import" href="/bower_components/video-labelging/video-labelging.html">


Add the control label in the place you want the control to appear, wrapped in a div:

    <div style="width:800px">
        <video-labelging id='video-labelging'></video-labelging>
    </div>

There is a demo index.html page in the demo folder with an example of hosting the control and interacting with a backend service to get/persist data.

**Control API**
The control recieves and sends data from/to the host.   

Initial Data -   
The following properties must be populated:

   1) videoduration - number, for example 30.07  
   2) videowidth - number, for example 420  
   3) videoheight - number, for example 240  
   4) framerate - number, for example 29.97  
   5) positionshape - string, can be "x", "rectangle" or "circle"  
   6) positiontype - string, can be "point" or "area"  
   7) positionsize - number, for example 20 (in pixels)  
   8) multipositions - string, can be "0" or "1" 
   9) inputlabelsarray - a string array of the possible labels, for example - ["horse", "bird]
  10) inputFrames - an object containg all the positions of this video (That have been created at an earlier time).
      Example: The object is a dictionary, the frame number is the key. Each frame has a collection of positions.  
      In this example we see data for frames 17, 23 and 41.  
      ![Alt text](images/frames1.png?raw=true "Title")  
      Expanded- each position is an obect with coordinates and a labels array.  
      ![Alt text](images/frames3.png?raw=true "Title")
  
Call these properties on the control, for example:

    var videolabelging = document.getElementById('video-labelging');
                
        videolabelging.videoduration = data.video.DurationSeconds;
        videolabelging.videowidth = data.video.Width;
        videolabelging.videoheight = data.video.Height;
        videolabelging.framerate = data.video.FramesPerSecond;
        ,,, 
      
  Finally, to load the control, set the src property to the URL of the video: 
 
    videolabelging.src = data.video.Url;

tagging Data -     
When a position is created or updated, the control fires an event called "onpositionchanged". Register to this event to get the data:

        document.getElementById('video-labelging').addEventListener('onpositionchanged', function (e) {...
The control sends **all** the positions and their labels in the current frame. The parameter e holds this data in e.frame:  

![Alt text](images/frames4.png?raw=true "Title")

