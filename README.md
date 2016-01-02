# FlashBalknaTestPlugin
Exemplar plugin for FlashBalknaModel:
Plugin can be zip/jar file downloaded from net (or created by you) or any directory inside your plugin(see a bit lower) directory containing the files organized in same way.
Each plugin consists from  trainingsXXX.xml, exercisesXXX.xml and cyclesXXX.xml  and optional images. Ideally saved in directory org/fbb/balkna/data as demonstrated in this example:
└── org
    └── fbb
        └── balkna
            └── data
                ├── exercises2.xml
                ├── cycles2.xml
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
All together is then packed as zip (or in named directory). And you can upload it to any internet or local destination and use in your application:
 - by copying to
  - pc:  XDG_CONFIG_DIR/FlashBalkna/plugins (which goes to HOME/.config/FlashBalkna/plugins mostly)
  - android who knows...
 - from tui - temporally usage via get URL
 - from gui in training settings, use text field and download button. 

** Create custom exercises and trainings and cycles**
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

Each exercise the looks like scripplet below. ( images and descriptions  elements are only obligatory)
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


*Cycles*
Cycles serves for long time training tracking and morphing.
When time shift is on, it is applied on top of each exercise in each training so be careful.
They are saved (similarly to trainings and exercises) as cyclesXXX.xml
They are always wrapped by
<cycles>
And each  cycle is in 
	<cycle>
with
		<id>cycle2_1</id>
		<names>
			<name>Classical iterations growing - 4 x 4 - easy</name>
			<name locale="cs">Klasické navyšování iterací - 4 x 4 - lehký</name>
		</names>
		<descriptions>
			<description></description>
			<description locale="cs"></description>
		</descriptions>
        <!-- voulenteer image
        <images>
            <image>imgX.jpg</image>
            <image>imgY.jpg</image>
        </images>
        -->     
As training is wrapping exercises, cycle is wrapping trainings. So you write all trainng into
		<trainings>
wrapping element and then for each training
			<training>
You ahve to specify  its original id
				<id>fbb1</id>
and the changes you wont to apply to this training into element  changes eg like:
				<changes>
					<iterations>0.6</iterations> <!--floating number, part of iterations -->
				</changes>
			</training>
			<training>
				<id>fbb1</id>
				<changes>
					<iterations>0.7</iterations>
				</changes>
			</training>
			<training>
				<id>fbb1</id>
				<changes>
					<iterations>0.8</iterations>
				</changes>
			</training>
			<training>
				<id>fbb1</id>
				<changes>
					<iterations>0.9</iterations>
				</changes>
				<suggestbreak>
                                    <descriptions>
                                        <description>day or two</description>
                                        <description locale="cs">Den či dva</description>
                                    </descriptions>
                                </suggestbreak>
			</training>
                        ....
Now - changes may contain
                                    <time>Z</time>
                                    <pause>Y</pause>
                                    <iterations>X</iterations>
                                    <rest>W</rest>
But unlike defaults and overrides, those are FLOATING numbers. Eg 0.5 or 1  or 1.3 which multiple (overrided) exercise's value. (the time shift, if any, si then calculated on top of it)
So you can  pretty easily write training which go slowly, but steadily up, and allows also stressing jumps.
The optional <suggestbreak></suggestbreak> is hint, which will write to documentation that you, as author, do recommend rest. Define in documentation what you mean by rest. Normal is day, but may think about week or just few hours.
The descriptions elements in suggestbreak is mandatory. But as always, if presented, it have to have at least one description element (the one without locale attribute).

** Localizations **
You have probably noted, that each name/description can be saved in various localizations like:
            <names>
                <name>open hang on big ones on top</name>
                <name locale="cs">otevřený úchop, velký madla za balknou</name>
            </names>
or
           <descriptions>
                <description>not yet</description>
                <description locale="cs">nedodáno</description>            
            </descriptions>

The sentence  WITHOUT attribute LOCALE is mandatory, and is used when no localized value is found (for set/detected locale)


** Sound Plugins **
Your plugin can have complelty custom sounds.
Unlike images and xml files, the sounds MUST honor the directory structure ( I guess sound plugins will be much less occuring)  of:
org/fbb/balkna/data/soundpacks/SOUNDPACK_NAME/file.wav
exemplar-plugin
└── org
    └── fbb
        └── balkna
            └── data
                ├── cycles1.xml
                ├── cycles2.xml
                ├── exercises2.xml
                ├── imgs
                │   └── exercises
                │       ├── push1.jpg
                │       ├── push2.jpg
                │       ├── push3.jpg
                │       ├── push4.jpg
                │       ├── push5.jpg
                │       ├── push6.jpg
                │       ├── push7.jpg
                │       ├── push8.jpg
                │       ├── push9.jpg
                │       ├── pushRest1.jpg
                │       └── pushRest2.jpg
                ├── soundpacks
                │   └── fbbhorror
                │       ├── 1.wav
                │       ├── 2.wav
                │       ├── 3.wav
                │       ├── change.wav
                │       ├── endChange.wav
                │       ├── endPause.wav
                │       ├── endRun.wav
                │       ├── halfPause.wav
                │       ├── halfRun.wav
                │       ├── halfSerie.wav
                │       ├── halfTraining.wav
                │       ├── lastExercise.wav
                │       ├── lastSerie.wav
                │       ├── start.wav
                │       ├── threeQatsPause.wav
                │       ├── threeQatsSerie.wav
                │       ├── threeQatsTraining.wav
                │       ├── threeQats.wav
                │       └── trainingEnd.wav
                └── trainings2.xml
The filenames must be EXACTLY the same as in  main application (or in this exmaple) If you do miss some, the default sound of this name will be played.
If you do not wont some, just create silent track of the name you do not wont to play:)

