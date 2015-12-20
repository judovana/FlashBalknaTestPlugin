# FlashBalknaTestPlugin
Exemplar plugin for FlashBalknaModel:

Each plugin consists from  exercisesXXX.xml, exercisesXXX.xml  and optional images. Ideally saved in directory org/fbb/balkna/data as demonstrated in this example:

└── org
    └── fbb
        └── balkna
            └── data
                ├── exercises2.xml
                ├── trainings2.xml
                └── imgs
                    └── exercises
                        ├── push1.jpg
                        ├── push2.jpg
                        ├── push3.jpg
                        ├── push4.jpg
                        ├── push5.jpg
                        ├── push6.jpg
                        ├── push7.jpg
                        ├── push8.jpg
                        ├── push9.jpg
                        ├── pushRest1.jpg
                        └── pushRest2.jpg

Where XXX is unique ID of you plugin. And should be used as prefix and suffix in all IDs in your plugin (not honored in this example)
You do no need to honor the director structure, but it is recommended.
All together is then packed as zip. And you can upload it to any internet or local destination and use in your application:
 - by copying to
  - pc:  XDG_CONFIG_DIR/FlashBalkna/plugins (which goes to HOME/.config/FlashBalkna/plugins mostly)
  - android who knows...
 - from tui - temporally usage via get URL
 - from gui in training settings, use text field and download button. 

** Create custom exercises and trainings **
Editor is coming! But for now...
exercisesXXX.xml, exercisesXXX.xml are descrabing - first individual exercises, second how those exercises are to be trained


*exercises:*
Are always wrapped in <exercises> and <exercises-set>. Thats because each set may have defaults. The only reason for defaults i staht you may be lazy to copypaste them all around:
 <exercises-set>
        <set-defaults>
            <time>10</time> <!--seconds -->
            <pause>15</pause> <!--seconds -->
            <iterations>10</iterations>
            <rest>60</rest> <!-- rest (default) after whole exercise-->
        </set-defaults>
        <exercise>
....

Each exercise in this execise-set will have default time 10 seconds, pause between trainings 15 and 10 iterations (with rest before another exercises 60s) unless it owerrides it.

Each exercise the looks like scripplet below. (individual image and description  elements are only obligatory)
        <exercise>
            <id>a01</id> <! unique id among all exercises -->
            <names>
                <name>open hang on big ones on top</name>
                <name locale="cs">otevřený úchop, velký madla za balknou</name> <!-- localized name used when user's locale matches. otherwise default name is used-->
            </names>
           <descriptions>
                <description>not yet</description>
                <description locale="cs">nedodáno</description>            
            </descriptions>
            <images>
                <image>%{id}.jpg</image> <!-- %{id} mark is replaced by id-->
                <image>pushRest1.jpg</image> <!-- another image .. or no images..-->
            </images>
            <defaults> <!-- overrides set-defaults>
                <time>10</time> <!--seconds -->
                <pause>15</pause> <!--seconds -->
                <iterations>10</iterations>
                <rest>60</rest> <!-- rest (default) after whole exercise-->
            </defaults>
        </exercise>

*trainings:*
Again, they starts with
<trainings>
    <training>
        <id>fbb1</id>
        <names>
            <name>FBB training level easy</name>
            <name locale="cs">FBB lehký trénink</name>
            ......
        </names>
        <descriptions>
            <description>not yet</description>
            <description locale="cs">nedodáno</description>            
            ....
        </descriptions>
        <images>
            <image>img5.jpg</image>
            .....
        </images>
        <exercises>
            <exercise>
            ........
Where id, name, description and image have same meaning as for exercise and same mandatoriness. In the examples you may see that  training specific  images were not exactly great idea.
The important part is wrapper <exercises> which contains ordered (in order of presence) list of individual <exercise>.
In simple world the exercises may look like
        <exercises>
            <exercise>
                <id>a01</id>
            </exercise>
            <exercise>
                <id>a09</id>
            </exercise>
            <exercise>
                <id>a10</id>
            </exercise>
        </exercises>
Where id is pointer, to exercise defined (and marked by id) in some exercisesXXX.xml.
However world is not so simple, so each exercise may declare overrides like:
            <exercise>
                <id>b08</id>
                <overrides>
                    <time>15</time>
                </overrides>
            </exercise>
or
           <exercise>
                <id>EX1_01</id>
                <overrides>
                    <!--any field from exercise can be overriden here-->
                    <pause>15</pause>
                    <rest>45</rest>-->
                    <time>6</time>
                    <rest>150</rest>
                </overrides>
            </exercise>
So final result will not use default value from exercises.xml but the ones inside overrides from trainings.xml (first example only time, second all)

Also in addition, each <exercises> MAY be wrapped by <exercises-set count="X">
   <exercises-set count="5">
       <exercises>
            <exercise>
                <id>a01</id>
            </exercise>
            <exercise>
                <id>a09</id>
            </exercise>
            <exercise>
                <id>a10</id>
            </exercise>
        </exercises>
    </exercises-set>

Which have he same result, as if you copy paste content of exercise-set X times.
