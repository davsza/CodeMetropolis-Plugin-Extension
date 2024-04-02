# codemetropolis-plugin-extension
University assignment for a software called CodeMetropolis

# This repo contains some of the finest solution for an isssue for the CodeMetropolis project

CodeMetropolis is a software visualisation tool. It can create a Minecraft world using the values of source code metrics and the structure of the source code. Thus explore the inner structure of the program or compare several source code elements is easier. CodeMetropolis uses city metaphor for the visualisation of code elements like classes, functions or attributes. The city metaphor is one of the best known metaphors in software visualization. In this metaphor the source code components are represented as a part of a generated city, for example classes represented as buildings or methods represented as floor.
It is a set of command line programs, connected into a single toolchain and a couple of supporting plug-ins and scripsts. The first tool is the CDF Converter Tool. This tool creates properties for each item from a graph file, for exmaple functions or methods. The second tool is the Mapping Tool, which processes the output XML file of Converter Tool, assigns metrics to objects of the metropolis and generates an ouput XML that is ready to be used by the Placing Tool. The third tool is Placing Tool. This tool creates the city layout and generates an output XML that is ready to be used by the Render Tool. The last tool is Render Tool. This tool proccesses the ouput XML file of Placing Tool and creates a virtual city in a Minecraft world.
CodeMetropolis tools use XML files to communicate with eachother. CDF Converter Tool produces an XML file from the graph file of the source code, then Mapping Tool using the previous XML file generates an ouput XML file which is the input file for the Placing Tool. Then the Placing Tool generates an output XML file for the Rendering Tool. These last two XML files use the same format defined in an XML Schema.

https://codemetropolis.github.io/CodeMetropolis/
https://github.com/codemetropolis/CodeMetropolis

# Related task: https://github.com/codemetropolis/CodeMetropolis/issues/263
"Currently, it is hard to know the sub-buildings' names of the current building within the Minecraft world.
Create a command that allows the user to list all of the names in the current building. The list should be visible only for the current player (who issued the command). The list of the names should be retrieved from the output of the placing phase. The path to the file should be specified in the properties of the Spigot plugin. If the file is not available, a constructive error message should be displayed for the players when they issue the command."

The task has been evaluated and given a 5 (A+). It has also been granted a colloquium grade.
The project was uploaded afterwards, completion date: May 2022
