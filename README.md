Introduction
This application is a C# application capable of playing audio files using WMP SDK. The idea here is to use the bare minimum playback functionalities from WMP SDK and create our own business logic and UI on top of that. Although it seems like an overkill since the WMP SDK provides facilities for playlist management and controlling the playback, doing everything by ourselves gives us more control and more learning (the most important aspect of this exercise).

Background
In my previous job around 4 years back, I wrote an audio playback engine in C++ using DirectShow. The project made me curious to learn about other audio playback technology used. Since then, I have written a lot of applications that are capable of playing audio files on Windows using different technologies. I have audio players written in C++ and C#. I have audio players using technologies like DirectShow, ManagedDirectX, WmpSDK and XNA framework. I have now decided to put all these applications with some brief overview on CodeProject. This article mentions one of my applications which is written in C# and uses WMP SDK for playback(other versions will be put here soon).) I will now briefly discuss about the application architecture and the code I have written for writing this application.

Using the Code
First of all, let us discuss the business objects that are required in any audio player irrespective of the underlying playback technology. We will define classes for "Song", "Playlist" and the user interface which are independent of the underlying playback technology. Once we define these classes, they can be reused with any playback technology just by writing a suitable adapter to interface with the playback technology.

Creating a Song Object
First of all, we need a class that will represent an audio file. I have created a Song class for the same.

Song Class
This class represents an audio file. The "Name" property represents the name of the audio file. This will be used to display the song in "Playlist" and/or "currently playing" area. Since the user can add the same song multiple times in a playlist, we needed a way to differentiate multiple entries of the same song so the song class also contains an ID property. So if a user adds the same song multiple times, the "Name" and "Path" properties for both the entries will be the same but they can be identified by using their ID property. (NOTE: See the source code for implementation.)

Creating a Playlist Object
Now let us create a class that will keep a list of songs to be played, i.e., Playlist.

Playlist Class
The playlist class contains methods to add/remove/clear songs in the playlist. it contains mechanism to keep a track of the current playing song. It has methods to retrieve the current/next/previous items from the playlist. The user of these two classes (perhaps the UI) will have to create the playlist and add files in the playlist. The playlist class will then keep the user added files as a List of "Songs", a class we created, and mechanism to perform operations on this list of songs. The UI is completely oblivious of the Song objects and can simply add "file paths" to the playlist and the playlist will take care of the rest. (NOTE: See the source code for implementation.)

Creating Playback Engine i.e. Talk to WMP SDK for Playback
Now we have to write an adapter class that will talk to the WMP SDK and let us do the actual playback. The wrapper class will look like this:

Player Engine Class
This class contains the handle to the WMP SDKs player class. It provides functions to talk to WMP SDKs control functions like play/pause/stop/resume/attach new media. This class also contains an event which the UI can subscribe to and keep itself updated with the current playing status and stats. This class also keeps track of current playing status of the engine which is implemented as a member variable named "curStatus_". This status could have possible values implemented as:

Status emum
Now, we have the core functionality required to perform the playback. Now, we can go ahead and start thinking about the user interface and start writing it. (NOTE: See the source code for implementation of CORE functionalities.)

Creating the User Interface
As for the user interface, we just have to have a user interface that contains UI elements for:

Adding Song to playlist
Removing Song from playlist
Clearing the playlist
Playing a song
Controlling playback, i.e., stop/pause/resume
Perform playlist navigation, i.e., next/previous
Display current playback status
Display Metadata, i.e., current position, duration of song
I created a simple user interface with all these UI elements and got them to work (NOTE: See the source code for implementation).

I also tried to make this UI occupy minimum area on the screen. For that, I created a Drawer like interface in which the user will only have the drawer handle on screen and can use this handle to pull out the drawer to see the full UI and perform some operations. Once he is done, he can use the handle again to hide the full UI.

Here are the classes that I created to achieve the above functionality (NOTE: See the source code for implementation):

ui classes
The drawer form is the main form that talks to the playlist and player engine. The handle form is the form that is visible on screen to draw/undraw the drawer form. The sliding controller is the class that keeps track of the positions of drawerform and handle form. This class is also responsible for letting the user drag the handle on screen and reposition the drawer accordingly (NOTE: See the source code for implementation).

This covers the basic architecture of this music player. This code is completed in 14 person-hours by me. So this is not written in the most elegant manner and it also lacks a lot of functionalities from an audioplayer's point of view. Some of the things that I would like to do in future are:

Make the event mechanism asynchronous (right now it is synchronous, which is pretty bad and quite a big problem)
Have a volume controller to change volume on All Windows (including xp, vista and 7) 
Have a playlist controller to have repeat/shuffle functionalities
Be able to save and load playlists in standard formats
Have more playback engines using (Directx/XNA/MDX) and dynamically choosing the best one based on the current system configuration
Save the user settings/playlist on disk so that we can remember them on the next run
History
23 March 2013: Volume Controller working on windows xp machines 
Version 1: Using WMPSDK with bare minimum functionalities for the user
