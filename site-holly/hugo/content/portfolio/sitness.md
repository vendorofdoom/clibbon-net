+++
draft = true
image = "images/sitness/turtle_beach.png"
showonlyimage = true
title = "Sitness"
+++

**Sitness** is a VR fitness application for seated exercises, implemented in Unity.

<!--more-->


Sitness was a group coursework project created for our Virtual Reality module. The team included [Mario "Bato" Balvanera](https://bato.crd.co/), PJ Davy, [Sean Hagstrom](http://seanhagstrom.com/), and myself. Each team member performed a variety of roles within this project. My personal contribution was mostly design, programming, testing, and a little bit of 3D modelling. This page will give an overview of the project and discuss my personal contributions and reflections in more detail.

{{< youtube id="HU4XhaEbVYs" title="A walkthrough video of Sitness" >}}

---------------------------
+ [Concept](#concept)
+ [Design](#design)
+ [Implementation](#implementation)
+ [Reflections](#reflections)
+ [Individual Additional Feature](#individual-additional-feature)
---------------------------

### Concept

Sitness is a VR application designed to encourage physical activity through "exergames" (gamified exercises). Sitness focuses on seated, arm-based exercises making it different to many existing VR fitness applications. We chose seated VR as a creative constraint to make the application a more comfortable VR experience and accessible to a wider audience.

### Design

We designed three "exergames" that could form a basic routine: a warm up, a main exercise, and a cool down.

For each "exergame" we devised a metaphor to disguise the movement as a fun and powerful VR interaction. Lighting, sounds and visuals are used to convey the mood of each exercise and game elements keep the player engaged.

The exercises we chose to implement came from Paul Eugene, a fitness instructor, who posts many chair fitness routines on his [YouTube channel](https://www.youtube.com/c/PaulEugene/featured).

#### Storyboards

After deciding as a team which exercises we wanted to include, I created the basic storyboards and gifs to describe the exericises and their related VR interactions. Additionally, I proposed a design for the diegetic main menu interface. 

##### Warm Up "Cloud Generation"

For the first exercise, the warm up, we chose a gentle arm stretch.

{{< figure src="/images/sitness/arm_stretch.gif" alt="Moving image of Holly stretching arms out to the side and back in again in VR headset" width="300">}}
  
{{< line_break >}}

The metaphor we designed to go along with this exercise is one where the player stretches their arms wide to create a cloud and then moves their arms back in again to "swoosh" the cloud away into the valley below.

![Design for the warm up exercise](/images/sitness/cloud_generation_design.png)


##### Main Exercise "Planet Walker"

For the main exercise we wanted something that would raise the heart rate a little. We decided to go with an "arm swing" motion as if you were running or walking with your arms only.

{{< figure src="/images/sitness/run.gif" alt="Moving image of Holly making running arms in VR headset" width="300">}}

{{< line_break >}}

The metaphor for this exercise is that you are keeping the planet spinning by walking on top of the world! As you swing your arms, the planet rotates beneath you.

![Design for the core training exercise](/images/sitness/planet_walker_design.png)

##### Cool Down "Beach"

For the final cool down exercise, we wanted to provide the player with another gentle exercise to wind down from the routine. We chose a simple arm raise and lower exercise for the cool down.

{{< figure src="/images/sitness/arm_raise.gif" alt="Moving image of Holly raising and lowering arms in VR headset" width="300">}}

{{< line_break >}}

To align with the arm raising/lowering motion, I designed a beach scene where the player's movement controls the raising and lowering of the tide.

![Design for the cool down exercise](/images/sitness/beach_cool_down_design.png)


##### Main Menu

For the main menu I designed a diegetic interface in the form of a planet which the player can grab and rotate to navigate our "world of exergames". The player can use the trigger button to select a exergame.

![Design for the main menu interface](/images/sitness/main_menu_design.png)

### Implementation

#### 3D Modelling

I designed the initial layout and created some simple models for the beach scene in Maya. Considering that the player should be seated for our VR application, I chose a cove style layout for the beach scene to draw the player's gaze forwards out to sea.  The scene was enhanced further by [Bato's](https://bato.crd.co/) 3D models and animated guide, as well as lighting and sounds added by PJ. 

{{< figure src="/images/sitness/beach_cove_design.png" alt="Design for the beach scene layout" >}}

*A sketch of the cove scene layout, the cove model, and the lighthouse on a headland model*

{{< figure src="/images/sitness/tree_models.png" alt="A trio of tree models" >}}

*Three, low-poly style trees I modelled for the beach scene*

#### Programming

[Sean](http://seanhagstrom.com/), PJ and myself worked on the various programming aspects of this project. You can find a copy of the project code [here](https://github.com/vendorofdoom/vr-sitness-individual).

##### Warm Up "Cloud Generation"

{{< line_break >}}

{{< figure src="/images/sitness/gen_clouds.gif" alt="A gif of the cloud generation">}}

*Create clouds by stretching your arms apart, "swoosh" clouds away by bringing your arms back together*



I worked with [Sean](http://seanhagstrom.com/) to design and implement the cloud generation interaction for the warm up scene. We spent a lot of time refining the logic that controls the position and rotation of the cloud "spawn" point relative to the player's controllers.

The script was written in a way to make the parameters of the interaction easily editable, which was useful when asking the other teammates to playtest and help calibrate the interaction.

I added haptics to this scene to simulate "resistance" when stretching the arms apart to create a cloud. I also added some incremental haptic feedback when the hands are moving together to help the player "feel" the cloud swooshing away.

[CloudPrototype.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/CloudPrototype.cs)

##### Cool Down "Beach"

{{< line_break >}}

{{< figure src="/images/sitness/water_level.gif" alt="A gif of the beach water level rising and falling with hand movements">}}

*Raising and lowering the tide by raising and lowering your arms.*

I merged my script which handled moving the water level up and down in relation to the player's hand movements with PJ's script that handled the beach sounds and hand reach calibration.

{{< line_break >}}

{{< figure src="/images/sitness/beach_treasure.gif" alt="A gif of the player using their gaze to discover treasure on the beach">}}

*Using gaze interaction to discover treasure on the beach!*

To add some game elements to the beach scene I implemented the code to spawn objects randomly at one of the pre-defined locations on the beach. This script was written in a way to allow [Bato](https://bato.crd.co/), the team's artist, to easily configure the array of objects to be spawned and their possible locations. The code that places the beach objects use a technique shown to me by PJ where a ray is projected down at the beach and the object is place upright in alignment with the normal of the surface that the ray hit.

These spawned objects each had the gaze interactable script attached meaning that they would disappear after being looking at for a specific amount of time by the player. This gaze interaction was implemented by casting a ray from the players head position forwards into space, if this ray hit a gaze interactable then its lifetime would decrease until it reaches zero, at which point is destroyed and the beach object spawner is told to spawn a new object.

[CoolDown.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/CoolDown.cs)  
[BeachObjectSpawner.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/BeachObjectSpawner.cs)  
[GazeInteractable.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/GazeInteractable.cs)  
[GazeInteraction.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/GazeInteraction.cs)

##### Main Menu and Scene Change Logic

{{< line_break >}}

{{< figure src="/images/sitness/main_menu.gif" alt="A gif of the main menu planet">}}

{{< line_break >}}

I added haptics to the main menu scene so that the player could "feel" when they were touching the planet or an exercise location. I also added script to have the UI text orient itself suitably to still be readable to the player as they rotate the planet. I added a general scene change script to do the screen fade and scene changes.

[GrabRotate.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/GrabRotate.cs)  
[MenuTextDirection.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/MenuTextDirection.cs)  
[SceneChange.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/SceneChange.cs)

### Reflections

The most enjoyable part of this project for me was designing the scenes and exercise metaphors, and then figuring out with my teammates how to implement the custom VR interactions. Creating custom interactions gave us the opportunity to spend more time putting the game maths we've been learning to good use.

I found haptics to be unexpectedly valuable, initially I was just adding them for completeness sake but I soon realised they have a subtle but markedly positive effect on the VR experience. This was particularly noticeable in the main menu scene where adding a short vibration when the player's controller intersects with the planet really helped to guide their movements.

It was challenging at times to understand whether a player was making a certain body movement without excluding people of different sizes and mobilities. A lot of time was spent between me and Sean playing around with the various mechanics, but in the end we weren't super strict in confining the player to certain movements and chose to focus more on the gameplay elements instead.

Working on this project I learnt more about the limitations of developing for the Quest 2, particularly in regards to the precision of the z-buffer when trying to render a large scene like the cloud generation scene where there are mountains far away in the distance.

I felt our team had a nice variety of skillsets which helped us to create a well-rounded VR experience. PJ added polish to each scene with custom sounds, thoughtful lighting and an informative UI system; Sean worked tirelessly to iron out any problems with the VR interactions and gameplay mechanics; and Bato brought the Sitness world to life with his charming 3D models and animations.

Overall, I am really pleased with what we've managed to create. On multiple occasions, after presenting our work, we received feedback that we should consider making our application available via [SideQuest](https://sidequestvr.com/). This is something I would certainly consider but not until we've playtested with a wider audience!

### Individual Additional Feature

#### Guidance System

In addition to our group work, we were asked to add one feature individually to further develop our concept. Based on the feedback we received from demoing our work, I chose to implement a simple guidance system to help players keep in time with the guiding avatar's movements.

{{< line_break >}}

{{< figure src="/images/sitness/cloud_gen_guidance.gif" alt="A gif of the warm up scene guidance system">}}

*Follow the guides to generate a cloud and "swoosh" it away!*

The guidance system takes the form of a moving sphere for each hand. The movements of these guiding spheres are tied to animation events in the guiding avatar animation. I added a guidance system to both the cloud generation warm up scene and the cool down beach scene. In addition to the moving spheres, there are controller button icons which can be displayed inside each sphere to help the player understand which buttons they need to press during an interaction. E.g. In the beach scene some initial configuration is required using the X and Y buttons.

{{< line_break >}}

{{< figure src="/images/sitness/beach_guidance.gif" alt="A gif of the cool down scene guidance">}}

*Configure the water level using the X and Y buttons*

I chose not to implement the guidance system for the planet walker scene for a few reasons: firstly, the guiding avatar is hidden during the exercise and replaced by a guiding moon and secondly, the arm swing exercise is not as timing based as the warm up and cool down exercises.

To allow the player to enable/disable the guidance system based on their preference, I added a moon to the main menu scene which can be selected to toggle the guidance system on/off. The chosen guidance system setting is stored in the player's [PlayerPrefs](https://docs.unity3d.com/ScriptReference/PlayerPrefs.html) so that it persists within each scene.

{{< line_break >}}

{{< figure src="/images/sitness/moon_o_settings.gif" alt="A gif of the main menu moon that toggles the guidance system on and off">}}

*Click the moon to toggle the guidance system on/off*

For the UI elements I created a simple fresnel shader for the guiding spheres and some custom sprites for the controller icons in a font that matched our existing UI.

The two main challenges I faced whilst implementing the guidance system were: 1. trying not to clutter the scene, and 2. keeping the guiding spheres within the field of view so the player can concentrate on the motion without having to move their head around too much. In terms of future development, I did find some of the exercises more challenging when trying to keep in time with the guiding avatar so perhaps some exercise adaptability settings are required to make Sitness more appealing to a wider audience. 

[CloudGenGuidance.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/CloudGenGuidance.cs)  
[CoolDownGuidance.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/CoolDownGuidance.cs)  
[GuidanceSettings.cs](https://github.com/vendorofdoom/vr-sitness-individual/blob/main/Sitness/Assets/Scripts/GuidanceSettings.cs)


