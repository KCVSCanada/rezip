# ReZip and ZipDoc Fork

This fork was created to make it easier for KCVS team members to setup ReZip and ZipDoc on team computers. This is also primarily intended to add .h5p processing for our upcoming project.

Rezip - For more efficient Git packing of ZIP based files.

ZipDoc - A Git `textconv` program to dump a ZIP files contents as text to stdout.

## Installation

This program requires Java JDK 14 or newer.

Begin by installing the current version of the Java Development Kit.

https://www.oracle.com/java/technologies/downloads/#jdk18-windows

After install ensure that the correct version of java is available in the System Path. To do this open a command prompt and enter: `java -version`.

```
C:\Users\<USERNAME>>java -version
java version "18.0.2" 2022-07-19
Java(TM) SE Runtime Environment (build 18.0.2+9-61)
Java HotSpot(TM) 64-Bit Server VM (build 18.0.2+9-61, mixed mode, sharing)
```

The default Java Runtime is JRE_1.8.\*, this version will not work the `git diff` command. If you have the JRE, and also want the JDK, you will need to set your system path to point to the newer JDK with a %JAVA_HOME% variable. I found this to be a useful reference: https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html.

Once you have a current Java installation set up, you will need to copy `ReZip.class` and `ZipDoc.class` into `~/bin`. It is possible you may need to create this folder. To do so navigate to your `C:\Users\<USERNAME>\` and create a new folder called `bin`.

Next we need to configure our git installation. Open a git bash window, then enter the following commands:

```
git config --global --replace-all filter.rezip.clean "java -cp ~/bin ReZip --store"
git config --global --add filter.rezip.smudge "java -cp ~/bin ReZip"
git config --global --replace-all diff.zipdoc.textconv "java -cp ~/bin ZipDoc"
```

The git filter and diff commands are now set up. Next we need to add a `.gitattributes` file to the root of the repo you are working with. We add the following attributes to `<repo-root/.gitattributes>`:

```
# MS Office
*.docx  filter=rezip diff=zipdoc
*.xlsx  filter=rezip diff=zipdoc
*.pptx  filter=rezip diff=zipdoc
# OpenOffice
*.odt   filter=rezip diff=zipdoc
*.ods   filter=rezip diff=zipdoc
*.odp   filter=rezip diff=zipdoc
# Misc
*.mcdx  filter=rezip diff=zipdoc
*.slx   filter=rezip diff=zipdoc
*.epub  filter=rezip diff=zipdoc
*.h5p   filter=rezip diff=zipdoc
```

Lastly, we need to open a git bash to the repo root and run the following command to help prevent unnecessary merge conflicts.

```
git config --add --bool merge.renormalize true
```
