Running Instructions:
---------------------
(note that these instructions assume that you are on Windows.  If you are using any other operating system, you may need to make changes to the documented commands and the "cb2xml.bat" file for compatibility)

1) Unzip the release into a suitable directory, preserving the folder structure

2) You should have Java installed (version 1.4 or later as the XML parser is required).  The java executable (java.exe on Windows) should be in your PATH

3) Change directory to the "bin" folder.  Type "cb2xml foo.txt >foo.xml" where foo.txt is the name of the target copybook file.  The resulting xml file will be called "foo.xml"  If you have downloaded and installed (unzipped) the CB2XML distribution, you can try converting the demo copybook by typing "cb2xml ../etc/sample.txt >../etc/sample.xml"

You can append the word "debug" to generate a detailed parse trace, in case you run into issues.  For example: "cb2xml foo.txt debug  >foo.xml"

Parser generation / Build Instructions (for developers wishing to modify cb2xml)
--------------------------------------------------------------------------------
You need the sablecc.jar library which you can download from the SableCC website (http://sablecc.org)  Although the current stable version is 2.18.2, you can choose to use the 3.X beta version which is also compatible with CB2XML.

There are 2 steps in the process 1) Generate the parser from the grammar file 2) compile the generated java files

The included ant build file has a "sablecc" target.  This generates the parser code using the sablecc.jar and also compiles all the generated code. For this target to run, sablecc.jar is expected in the "sablecc" directory.  Note that this ant target forces a clean of the generated code output directories.  This is recommended because this ensures consistency especially when you make drastic changes to the grammar file (scc/sablecc.scc)

Once you have executed the ant "sablecc" target, the ant "build" target, compiles and creates cb2xml.jar in the "lib" folder.