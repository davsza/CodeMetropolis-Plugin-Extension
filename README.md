## codemetropolis-plugin-extension
University assignment for a software called CodeMetropolis

## This repo contains some of the finest solution for an isssue for the CodeMetropolis project

CodeMetropolis is a software visualisation tool. It can create a Minecraft world using the values of source code metrics and the structure of the source code. Thus explore the inner structure of the program or compare several source code elements is easier. CodeMetropolis uses city metaphor for the visualisation of code elements like classes, functions or attributes. The city metaphor is one of the best known metaphors in software visualization. In this metaphor the source code components are represented as a part of a generated city, for example classes represented as buildings or methods represented as floor.
It is a set of command line programs, connected into a single toolchain and a couple of supporting plug-ins and scripsts. The first tool is the CDF Converter Tool. This tool creates properties for each item from a graph file, for exmaple functions or methods. The second tool is the Mapping Tool, which processes the output XML file of Converter Tool, assigns metrics to objects of the metropolis and generates an ouput XML that is ready to be used by the Placing Tool. The third tool is Placing Tool. This tool creates the city layout and generates an output XML that is ready to be used by the Render Tool. The last tool is Render Tool. This tool proccesses the ouput XML file of Placing Tool and creates a virtual city in a Minecraft world.
CodeMetropolis tools use XML files to communicate with eachother. CDF Converter Tool produces an XML file from the graph file of the source code, then Mapping Tool using the previous XML file generates an ouput XML file which is the input file for the Placing Tool. Then the Placing Tool generates an output XML file for the Rendering Tool. These last two XML files use the same format defined in an XML Schema.

https://codemetropolis.github.io/CodeMetropolis/
https://github.com/codemetropolis/CodeMetropolis

## Related task: https://github.com/codemetropolis/CodeMetropolis/issues/263
"Currently, it is hard to know the sub-buildings' names of the current building within the Minecraft world.
Create a command that allows the user to list all of the names in the current building. The list should be visible only for the current player (who issued the command). The list of the names should be retrieved from the output of the placing phase. The path to the file should be specified in the properties of the Spigot plugin. If the file is not available, a constructive error message should be displayed for the players when they issue the command."

The task has been evaluated and given a 5 (A+). It has also been granted a colloquium grade.
The project was uploaded afterwards, completion date: May 2022

## Implementation

Preparing the task is divided into two main parts:

# XML processing
The first is the processing of an xml file, namely placingToRendering.xml, which contains the exact position and size of the buildings in our city that will be created later. Its structure is hierarchical, i.e. it consists of individual elements and their children. An example of such a garden (Garden), which can have several buildings, can have Floors and Cellars.
Example:
```
<buildable id="da198328-d053-49ad-8ca0-295cd31291db" name="BuildableDepthComparator" type="garden">
  <position x="39" y="66" z="102"/>
  <size x="15" y="9" z="17"/>
  <attributes>
    <attribute name="flower-ratio" value="0.14285714285714285"/>
  </attributes>
  <children>
    <buildable id="0b82b458-68aa-4b8a-9160-9cabbcd7f29b" name="int compare(Buildable b1, Buildable b2)" type="floor">
      <position x="42" y="67" z="105"/>
      <size x="9" y="13" z="11"/>
      <attributes>
        <attribute name="external_character" value="metal"/>
        <attribute name="character" value="glass"/>
        <attribute name="torches" value="2"/>
      </attributes>
    <children/>
    </buildable>
  </children>
</buildable>
```
The file was processed using the Java org.w3c.dom interface. At first, we only used a burned-in position, we searched for the root node with which we had to work later. We implemented the search with a recursive method, which called itself until the appropriate Node was found, and we checked this with its position and size attributes. The given Node was suitable if our position (coordinates entered manually) falls between the position and position+size coordinates, both on the x and z axis. As soon as the element to be processed was obtained, we retrieved its children Nodes using another method.

# Player service
The second task is to return the desired sub-building list to the player. You have two options for this. You can either simply use the command, or make an additional request using the `-d` switch. Then not only the children of the given Node, but their children, and their children, etc... can be retrieved, which was also solved using a recursive function. If the user needs more items to appear in the chat than can fit (default 20 items), the listing is displayed as pages, between which the user can navigate by entering the appropriate page number after the command, provided that it exists.

## Usage

# Basic function
Pressing the letter `t` opens the chat window, in which the `/sub-building` command must be entered. In the event that the path of the xml file to be examined was not specified correctly in the config.yml file, the user will receive an error message. Otherwise, the plugin will return the names of the children of the building in the current position by default, and they will be visible in the chat. (if it is in a garden, then the names of the buildings, in the case of a square, the gardens, buildings, etc.) we can do it, e.g. `/sub-building 3`, getting page 3 of our list - provided page 3 exists. Of course, if we want to access a page that cannot be found, the player will receive an error message. (If no page number is specified, the first page will be displayed in default size, but it can also be accessed using the sub-building 1 command)

# Deep search
You may wonder what happens if the listed elements have additional children in the tree hierarchy. What happens if our file is structured something like the example below?
```
<building>
  <floor>
    <table>
    <chair>
  </floor>
  <ceiling>
    <lamp>
  </ceiling>
</building>
```
In basic mode, if we are in the building area, only the floor and ceiling elements would be displayed by the plugin. If we want to do this search deeper, we can do it with the `-d` or `-deep` switch. Its use is similar, it must be inserted as the first parameter after the command, followed optionally by the page number: `/sub-building [-d] [page number]`. Here, it is worth always entering a page number, since there will presumably be a much larger amount of data, which will not fit in its entirety on the chat.

# Other informations
If any argument is entered incorrectly, the player will receive an error message. For more help, you can use the `/help sub-building` command.
![image](https://github.com/davsza/codemetropolis-plugin-extension/assets/74209747/2f8156e0-fbfa-4ea6-b647-ca475961f7f1)
