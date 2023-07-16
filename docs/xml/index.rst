UI Rendering
============

Before getting to XML, it is important to discuss how Oblivion actually renders its UI.

All UI elements are flat 3D meshes with a simple texture applied. The meshes are physically rendered
close to the camera, with the integer depth value only separated by 0.008 Y units. This is also the
reason why the FOV setting in Oblivion.ini affects the UI. With sufficiently large integer depth
values, one can see the apparent issues with this method of rendering.

.. _xmlreference:


XML Reference
=============

XML (eXtensible Markup Language) is a markup language that Oblivion uses to store all the necessary
structural information for its UI. The language is based on the same specification as HTML, the
language used to render websites (what you are reading right now), so the syntax is very similar:

.. code-block:: Xml

   <menu name="MenuName">
	  <string> "Hi!" </string>
   </menu>

.. code-block:: Html

   <menu name="MenuName">
	  <string> "Hi!" </string>
   </menu>

Unlike HTML however, XML was designed to be built for any situation. As a result, there's specific
implementations for the project in mind. Oblivion has a very stripped down version compared to many
other XML implementations

Oblivion XML Limitations
------------------------
- CDATA is not supported
- XML headers/declarations and doctypes are not supported
- Namespaces are not supported
- Standard XML and HTML entities are not supported
- Attribute values must be placed in double-quotes not single-quotes
- Everything is case-insensitive
- Leading and trailing whitespace is universally ignored. This is contrary to how many vanilla
  files are formatted, so formatting is up to the author
- Whitespace is defined as any single-byte character with the hexadecimal value lower or equal
  than ``0x20``. This includes carriage return and tabs
- The only supported attribute for tile elements is "name". This attribute is required, causing
  crashes if missing
- Attributes not explicitly in this implementation should be avoided for this reason
- Only 128 bytes are allocated for tag names, including the null terminator, but this is not
  terminated. This will cause memory corruption if too long
- Only 4096 bytes are allocated for text content, including the null terminator. Text content is
  generally any attribute names, values, and plain-text inside an element
- All content in the file before the first ``<`` literal is ignored outright
- Numbers and strings are poorly discriminated. Numbers are numeric if the string of characters
  contains only digits, dashes, and periods, leading to ``-.-.-.-`` to be a valid number

XML Files
---------

Generally XML files are called "documents". For generic menus, the documents **must** be placed
somewhere in ``Data\Menus\Generic``. It is recommended to place your generic menu documents inside a
folder for your plugin name or your author name for clarity. Documents will be searched from this
directory if not prepended with ``Data\Menus\Generic``.

.. _prefabdocuments:

Prefab Documents
________________

It is possible to include documents inside another document with the ``include`` trait:

.. code-block:: Xml

   <image name="scroll_bar"/>
	  <include name="vertical_scroll_bar"/>
	  ...

Like generic menu documents, these must be placed inside a specific folder. For prefab documents it
is ``Data\Menus\Prefabs``, and will search inside this directory if not prepended. You cannot
include a file outside of this folder.

An included document imports all traits to the current tile.

XML Comments
------------

Comments in XML and similar languages are very different from most other languages. Since everything
is written into tags, the comments themselves need to be written into tags:

.. code-block:: Xml

   <!-- This is a comment -->

For posterity, the leading tag is: ``<!--`` and the closing tag is ``-->``. Comments can span as
many lines as you want. Oblivion will ignore all comments.

XML Types
---------

There are 3 types available:

:float: Any plaintext that contains only digits, dashes, and periods
:string: Any plaintext that doesn't meet the above
:entity: String of Latin characters, numerals, hyphens, and underscores enclosed between ``&`` and
		 ``;``. Used for engine values and booleans (e.g. ``&true;``)

XML Tiles
---------

Tiles are the main block element of an XML document that is what is rendered

Menu Tile
_________

This tile must be the root of the XML document as it controls what menu is rendered. This tile must
also contain the ``class`` trait, which corresponds to one of the menu types.

.. code-block:: Xml

     <menu name="MenuName">
       <class> &GenericMenu; </class>
     </menu>

This tile generally doesn't accept any rendering trait.

Rect Tile
_________

This tile is the most used tile. It is used to group other tiles for easier management, such as
position and visibility, as well as used for features such as lists. This tile can accept any of the
positional and rendering traits, but alpha and color should not be touched due to undefined
behavior. If you want a tile with solid colors, it is best to use a NIF or image tile.

Image Tile
__________

This takes a standard, Oblivion supported, DDS texture. Oblivion supports 3 (DXT1/BC1, DXT3/BC3, and
DXT5/BC5). DXT1 is used when there's either no alpha or for 1-bit alpha transparency in the texture.
For most image tiles, it is ideal to use DXT3 or DXT5 depending upon how alpha is used by the
creator.

Compared to how textures work for the gameworld, Oblivion uses different textures based upon the
ratio of the screen. Inside ``Data\Textures``, there are 3 folders for menu textures: ``Menus``,
``Menus50``, and ``Menus80``. When textures aren't found in either of the numbered folders, Oblivion
will use the texture in the un-numbered folder.

Oblivion renders different texture sizes solely by mip-maps. Menu textures thus *should not* have
mip maps. For programs such as Photoshop w/ DDS plugin or DDSOpt this should not be an issue, but
GIMP falsely renders mipmaps when they should be disabled.

Oblivion menus have broken texture filtering. You should not use textures that are too high
resolution if possible.

NIF Tile
________

While you can't put in arbitrary meshes due to missing alpha and materials properties on renderable
menu NIFs, you can use any regular Oblivion NIF. These are commonly used for animated bars, such as
the breath bar in vanilla.

``3D`` is a valid alias for this tile.

Text Tile
_________

This renders text. The text width can be determined automatically if the ``width`` tile isn't used.
You can also clip the text so it automatically wraps to the next line.

XML Traits
----------

Trait are elements of a tile that Oblivion uses in rendering the tile. While there are a few
traits that are used with all tile types, most are only possible to use on other tiles.

Generic Traits
______________

These traits are available to any tile:

:childcount: The tile's number of direct children
:child_count: The tile's number of direct children
:id: The tile's numerical identifier
:user#: Easily readable/writable traits for user manipulation
:underscore prefixed traits: In order to get past hardcoded trait limitation, any trait prefixed
							 with an underscore (e.g. ``_trait``) is considered custom

Behavior Traits
_______________

These affect the behavior of tiles, such as clicking:

:clickcountafter: Used but unknown behavior
:clickcountbefore: Used but unknown behavior
:clicked: If this tile's ``target`` trait was true and the tile has an ``id`` trait, this will get
		  set to 1 then 0 in the same frame to indicate that it was clicked
:clicksound: If this tile's ``target`` trait was true and the tile has an ``id`` trait, this will
			 play a sound based on the numerical value set for this trait:

1. ``UIMenuOK``: A clicking sound overtop a low drumbeat
2. ``UIMenuCancel``: A clicking sound
3. ``UIMenuPrevNext``: A slightly higher-pitched clicking sound
4. ``UIMenuFocus``: Silent
5. ``UIMenuTabs``: Silent
6. ``ITMBookPageTurn``: 
7. ``UISpeechRollover``: A quick, high-pitched clink
8. ``UISpeechRotate``: Something rotating into place, with several quick metallic clicks as it goes. A longer sound. Used for the Persuade minigame
9. ``UIQuestNew``: Quick cymbals and a drumbeat. Used when a quest starts
10. ``UIQuestUpdate``: The same as ``UIQuestNew``, but with some chiming at the end. Used when a quest updates
11. ``UIMessage``: A low drumbeat
12. ``MenuEnd``: 
13. ``MenuStart``: 
14. ``UIMenuBracket``: A metallic object sliding into place, with a high-pitched clink at the end
15. ``UIMessageFade``: Two high-pitched chimes
16. ``UIInventoryOpen``: Silent
17. ``UIInventoryClose``: Silent
18. ``UIPotionCreate``: A glass clink, followed by a low beat and the sound of bubbling. Used when you brew a poison or potion
19. ``DRSLocked``: 
20. ``UIMessage``: 
21. ``UIMenuCancel``: 
22. ``UIStatsSkillUp``: Drums. Used when a skill increases
23. ``SPLEquip``: A quick shuffling of papers over a low drumbeat
24. ``ITMWelkyndStoneUse``: 
25. ``ITMScrollOpen``: 
26. ``ITMScrollClose``: 
27. ``ITMBookOpen``: 
28. ``ITMBookClose``: 
29. ``ITMTakeAll``: 
30. ``ITMIngredientNothing``: 
31. ``ITMIngredientDown``: 
32. ``ITMSoulTrap``: 
33. ``UIArmorWeaponRepairBreak``: The sound of a Repair Hammer breaking after being used up
34. ``ITMBoundDisappear``: 
35. ``ITMGoldUp``: The sound of coins clinking together. Good for spending or receiving gold
36. ``UIItemEnchant``: A long, louder magical sound effect

:focusinset: The amount of value to shrink a focus box inward. Use on the tile you want focused
:listindex: This is used for lists, see :ref:`List Generation Functions <ListGeneration>`
:mouseover: If this tile's ``target`` property is true, this will be set to 1 when the mouse hovers
			over this tile and 0 at *any* other point of time. This is important to note when taking
			keyboard controls in mind. This trait can interfere with keyboard controls
:shiftclicked: If this tile's ``target`` property is true and this tile has an ``id`` trait, this
			   trait will be set to 1 then 0 in a frame when the Shift key is held while this tile
			   is clicked
:target: If set to true, this tile can be affected by mouse and keyboard
:xbutton Friends: When suffixed with an Xbox controller button (e.g. ``xbuttona`` for the A button
				  and ``xbuttonlt`` for the left trigger), this will interact with a tile if it has
				  the ``mouseover`` trait
:xdefault: Undefined behavior that seems to deal with mouse/keyboard controls. This is a Boolean and
		   integer value, and is generally set to true
:xlist: Determines whether a tile is contains a list or contains an item for a list. See :ref:`List Generation Functions <ListGeneration>`
:xup: How the tile should respond to movement when it has the ``mouseover`` state
:xdown: How the tile should respond to movement when it has the ``mouseover`` state
:xleft: How the tile should respond to movement when it has the ``mouseover`` state
:xright: How the tile should respond to movement when it has the ``mouseover`` state
:xscroll: Used to control scrolling of a scroll bar with a list. See :ref:`Scrolling a List <ScrollingaList>`

Box Rendering Traits
____________________

These traits are used in the direct rendering of most tiles:

:clips: Boolean value that if true, and has an ancestor tile who has a ``clipwindow`` trait that is
		set to true, will hide the parts of this tile that are outside said ancestor's boundaries
		This value does not propagate among child tiles. This tile only applies to ``image``,
		``rect``, and ``text`` tiles
:clipwindow: If set to any non-zero value, decedents will be clipped if they contain the ``clip``
			 trait
:depth: A numerical integer value of depth, not the actual value of depth as rendered by the game
		Higher depth values render above lower depth values. As menus are rendered in 3D, very large
		values will become disjointed from the camera
:depth3d: The same as the ``depth`` trait but using the real 3D values. One ``depth`` unit is -0.008
		  3D units in the Y axis
:listclip: If this trait is set to true, then this tile will not render. However, it can still
		   receive interaction. See :ref:`List Generation Functions <ListGeneration>`
:locus: A boolean value, if set to true then this tile will render from the nearest ancestor who
		also has a ``locus`` trait set to true
:height: The height of the tile in pixels. This trait is not set automatically for ``image`` tiles,
		 and while they are for ``text`` tiles you cannot set this trait yourself
:width: The width of the tile in pixels. This trait is not set automatically for ``image`` tiles,
		and while they are for ``text`` tiles you cannot set this trait yourself
:x: The position the leftmost side of the tile is moved to the right by
:y: The position the topmost side of the tile is moved down by
:visible: Boolean value that controls both rendering and interaction state. The value is 0 by
		  default for all tiles, and 0 is presumed true

Color Rendering Traits
______________________

These traits affect the color rendering of tiles. These affect the vertex colors:

:alpha: The alpha transparency with a numerical value inclusive between 0 and 255. Unset alpha is
		either 0 or 255, i.e. undefined, and tiles that can't take this trait behave the same
		Mostly used for ``image`` and ``text`` tiles
:red: Standard 0-255 color values. Mostly used for ``text`` tiles. ``image`` tiles behave strangely
	  and should be avoided
:green: Standard 0-255 color values. Mostly used for ``text`` tiles. ``image`` tiles behave
		strangely and should be avoided
:blue: Standard 0-255 color values. Mostly used for ``text`` tiles. ``image`` tiles behave strangely
	   and should be avoided

Image Traits
____________

These traits only apply to the ``image`` tile:

:cropx: Behaves like ``x`` trait, but crops the image in the x direction. This is applied *after*
		the ``zoom`` trait. ``cropoffsetx`` tile is an alias
:cropy: Behaves like ``y`` trait, but crops the image in the y direction. This is applied *after*
		the ``zoom`` trait. ``cropoffsety`` tile is an alias
:cropoffsetx: Behaves like ``x`` trait, but crops the image in the x direction. This is applied
			  *after* the ``zoom`` trait
:cropoffsety: Behaves like ``y`` trait, but crops the image in the y direction. This is applied
			  *after* the ``zoom`` trait

:filename: The filename for a file contained in ``Data\Textures\Menus``. Textures outside of this
		   folder do not work
:fileheight: The size of the file computed by the engine *after* the ``zoom`` trait has been applied
:filewidth: The size of the file computed by the engine *after* the ``zoom`` trait has been applied
:tile: Should this tile be "tiled" like a wallpaper. This happens if its height and width exceed the
	   presented height and width
:zoom: An integer numerical value that is divided by 100 to receive the zoom factor for this tile
	   If set to -1, this tile will scale to fit the boundaries without care for proportions. The
	   ``&scale;`` entity is an alias to -1 in this trait

Menu Traits
___________

These traits are unique to the ``menu`` tile, which itself is generally restricted to these
traits:

:class: The internal engine class for this tile. All vanilla menus have appropriate XML entities
:disablefade: A boolean value, that when true will prevent the fading animation
:explorefade: This is used, sometimes, in place of the ``menufade`` trait. It has unknown behavior
			  and appears to a float numerical value
:menufade: Amount of time it takes for the menu to fade in or out
:stackingtype: Unknown behavior

NIF Traits
__________

These traits only apply to the ``nif`` and ``3d`` tiles:

:animation: The name of the animation used within the NIF file
:filename: The filename of the NIF found in ``Data\Meshes\Menus`` or a relative path if ``Data\``
		   isn't prefixed

Text Traits
___________

These traits are unique to the ``text`` tile:

:font: An integer numerical value corresponding to the 5 possible fonts Oblivion can load each game
	   1 indexed. Values outside of this range will crash the game unless extra fonts are used
:isHTML: Non-functional to end users, will cause blank text. Interprets the ``string`` trait as HTML
		 by the executable
:justify: Specifies which direction the text will propagate from using the following entities:
		  ``&left;``, ``&center;``, ``&right;``
:pagecount: Non-functional to end users. How many pages are available in this HTML-formatted text
:pagenum: Non-functional to end users. What page is currently in view
:string: The text to be displayed. Numerical values are formatted with ``.0%f``, leading to
		 truncation
:wraplimit: How many lines to limit the text, more will not be displayed
:wraplines: How many lines to limit the text, more will not be displayed
:wrapwidth: The number of pixels in the X axis before text moves to the next line. All values are
			rounded down during rendering. Below 1 is treated as no width

XML Operators
-------------
Operators behave differently to other language due to how XML processes data. All values are stored
in a stack, that roughly corresponds to the structure of a XML document. Therefore, there are no
real variables and instead everything is based around the "current value" of the parsing. For
example:

.. code-block:: Xml

    <x>
        <copy> 4 </copy>
        <add> 4 </add>
        <mul> 8 </mul>
    </x>

This would return 64.

You need to put a value into the "current working value"  of the parser. That is then kept in
memory as successive operators are applied to the current working value.

Copy Operator
_____________

This puts the value of this element into the current working value CWV. If the CWV is a number
than this operator also has the special function of taking a trait with the structure of
``_trait_`` and suffixing the CWV to it. This can be used for switch cases, and is already in use
in the vanilla game to manipulate how the RepairMenu handles different cases (alchemy, self
repair):

.. code-block:: Xml

   <text name="menu_title">
	  <_title_1> Repair your gear </_title_1>
	  <_title_2> Pay a merchant to repair gear </_title_2>
	  <_title_3> Select an alchemy ingredient </_title_3>
	  <_title_4> Select an item to enchant </_title_4>
	  <_title_5> Select a soul gem to enchant with </_title_5>
	  <_title_6> Select a sigil stone </_title_6>

	  <string>
		 <copy src="RepairMenu" trait="user0" />
		 <copy src="me()" trait="_title_" />
	  </string>
   </text>

- the 0th case will not be used
- If a missing trait is looked for, 0 is returned
- a trait with an underscore does not work, it will parse until the first underscore
- switch cases only work on the first instance of the copy operator, it does not work during runtime

Other Operators
_______________

:abs: Returns the absolute value of the current working value

:add: Adds argument value to current working value and returns it

:and: Returns true if both the current working value and the argument value evaluate to true or to
	  false

:ceil: Returns the current working value rounded up to the nearest integer. The argument value is
	   added to the current working value before rounding

:div: Divides the current working value by the argument value. Can be a float

:eq: Returns true if the current working value is equal to the argument, otherwise false. This only
	 works on floating-point values, it will fail with strings

:floor: Returns the current working value rounded down to the nearest integer. The argument value is
		added to the current working value before rounding

:trunc: Returns the current working value rounded down to the nearest integer. The argument value is
		added to the current working value before rounding

:gt: Returns true of the current working value is greater than its value, otherwise false

:gte: Returns true if the current working value is greater than or equal to its value, otherwise
	  false

:log: Returns the base-10 log of the current working value. Argument value is unused

:ln: Returns the natural log of the current working value. Argument value is unused

:lt: Returns true of the current working value is less than the argument, otherwise false

:lte: Returns true of the current working value is less or equal to than the argument, otherwise
	  false

:max: Compares the argument to the current working value, and returns the larger of the 2 values

:min: Compares the argument to the current working value, and returns the smaller of the 2 values

:mod: Using the argument value, returns the modulo of the current working value

:mul: Multiplies the current working value with the argument value and returns it

:mult: Multiplies the current working value with the argument value and returns it

:neq: Returns true of the current working value is not equal to the argument, otherwise false

:not: Casts the argument to a boolean, inverts it, and returns the result

:onlyif: Returns the current working value if the argument value returns true, otherwise 0

:onlyifnot: Returns the current working value if the argument value returns false, otherwise 0

:or: Returns true of one or both of the current working value and argument value return true

:rand: Set the current working value to a random integer between 1 and the argument value,
	   inclusively

:ref: Only used for navigation

:sub: Subtracts the argument from the current working value

:src: This is an attribute, i.e. argument of an operator/trait /itself/, that allows you to target
	  some other tile and its successive traits:

.. code-block:: Xml

   <rect name="outer_box" />
	  <x> 200 </x>
	  <rect name="inner_box" />
		 <x> <copy src="parent()" trait="x" /> </x>
	  </rect>
   </rect>


.. _selectors:

Selectors
_________
These are trait "functions" used when using the ``src`` attribute of an operator or trait. They
correspond to some other value found in Oblivion's menu memory:

:child: Finds a value in the current tile's child. If an argument is provided, then the name of the
		tile will be matched to the argument. The last child or null is returned if no match is
		found

:last: Unknown behavior

:me: The current tile

:parent: The parent tile of the current tile

:screen: Holds the value of the screen's render dimensions. It is the menu root

:sibling: Finds a value of the current tile's siblings. If an argument is provided, then the name of
		  the tile will be matched to the argument. The previous sibling or null is returned if no
		  match is found

:strings: A special file that contains various strings for localization

