.. _listgeneration:

List Generation Functions
=========================

Generic menus in Oblivion can natively render and handle lists of items. These are used in many
areas by the vanilla game, such as the skill menu, but can be adapted to any need. This framework
provides a very simple interface for doing so.

Historically this aspect of generic menus was unused by generic menus due to the perceived
complexity. The only other conventional example is LINK by Maskar.

.. _gmfinsertarraylist:

GMFInsertArrayList
------------------

:Args: ``@string::sTile``, ``@string::sTemplate``, ``@array::aList``, ``@ref::rFunction``
:Function Args: ``@string::sTile``, ``@array::aList``, ``@int::iIndex``
:Info: Inserts the contents of ``aList`` into ``sTile`` using the template ``sTemplate``.

	   Without ``rFunction``, this function will only create the number of elements for the list.
	   ``rFunction`` is a user-defined function that provides the rendered information as well as
	   any side-effects intended. See below for examples.

	   Pass 0 for ``rFunction`` to not use ``rFunction``

GMFInsertCustomArrayList
------------------------

:Args: ``@string::sTile``, ``@array::aList``, ``@ref::rFunction``
:Function Args: ``@string::sTile``, ``@array::aList``, ``@int::iIndex``
:Info: Inserts the custom contents of ``aList`` into ``sTile``.

	   The array passed must contain a stringmap for each index, with each stringmap at least has
	   the key ``template`` which contains the template you wish to insert for this index.

	   Without ``rFunction``, this function will only create the number of elements for the list.
	   ``rFunction`` is a user-defined function that provides the rendered information as well as
	   any side-effects intended. See below for examples.

	   Pass 0 for ``rFunction`` to not use ``rFunction``

Inserting a List to a Menu Tile
-------------------------------

.. note::
   The following is simply the bare minimums to have a functioning list as a reference. This is not
   intended to be a copyable for end use

Lists are inserted into a ``rect`` menu tile. It is highly recommended that you make this tile
specific for your list, as it will simplify control of your list.

List ``rect``
_____________

.. code-block:: Xml

   <rect name="list_rect" />
	  <id> 100 </id>
	  <target> &false; </target>
	  <xdefault> &true; </xdefault>
	  <xlist> &xlist; </xlist>
	  <xscroll> <ref src="scroll_bar" trait="user5"/> </xscroll>
   </rect>

The traits here are all needed:

:id: In order for the list to be manipulated
:target: Must be ``&false;`` as this tile is not what gets targetted
:xdefault: Must be ``&true;``
:xlist: A special trait that tells the engine that this tile contains a list, must be set to
		``&xlist;``
:xscroll: Must be set to the scroll bar tile you are using, see :ref:`Scrolling a List <ScrollingaList>`

List Template
_____________

Templates are end of file XML elements that can be inserted as many times as needed into another
tile. For lists, this template should be a ``rect`` that contains the elements you wish to display:

.. code-block:: Xml

   <template name="list_template">
	   <rect name="list_item">
		   <id> 200 </id>
		   <target> &true; </target>
		   <clips> &true; </clips>
		   <listclip>
			   <copy src="me()" trait="listindex"/>
		   </listclip>
		   <y>
			   <copy> height </copy>
			   <mul>
				   <copy src="me()" trait="listindex" />
				   <sub src="scroll_bar" trait="user7" />
			   </mul>
			   <add> height </add>
		   </y>
		   <listindex> 0 </listindex>
		   <xdefault> &false; </xdefault>
		   <xlist> &xitem; </xlist>
		   <xscroll>
			   <copy src="me()" trait="listindex" />
			   <sub>
				   <copy src="scroll_bar" trait="user8"/>
				   <div> 2 </div>
				   <ceil> 0 </ceil>
			   </sub>
			   <add> 1 </add>
		   </xscroll>

		   <text name="list_name" >
			   <string> <copy src="parent()" trait="user1" /> </string>
		   </text>

		   <rect name="list_focus">
			   <include src="darn\focus_box.xml"/>
			   <visible>
				   <copy src="parent()" trait="mouseover" />
				   <eq> 1 </eq>
			   </visible>
		   </rect>
	   </rect>
   </template>

The rect needs the following to render at all:

:id: This must be set for the list to be interactable
:target: Must be set to ``&true;`` for the items to be targetable
:clips: A special trait that allows tiles to unrender when they're behind another regardless of
		depth, must be set to ``&true;``
:listclip: A special trait that determines when a list item gets clipped, must be set to this
		   special value TODO: fix
:listindex: Determines what position in the list this list item is, must be set to 0 here
:xdefault: Must be set to ``&true;``
:xlist: A special trait that lets the engine know that this tile is a list item, must be set to
		``&xitem;``
:xscroll: Determines how far the items should scroll, such as if items should keep scrolling even if
		  there's no more (leaving blank spaces). Keep to these values unless you want a different
		  value

The two other tiles are purely to display a basic text list. The ``text`` tile is self explanatory,
the second ``rect`` tile uses a prefab XML file to add a focus block to each item on mouseover.

Side-Effect Function
____________________

In order to maintain abstraction and simplicity, :ref:`GMFInsertArrayList <GMFInsertArrayList>`
allows a user-defined function to be passed that allows for *something* to happen to each list item.
The way this works is that during the loop of inserting the array contents into the menu, the passed
function can be passed if found will run for each of these contents in the loop. This is why the
arguments of the user-defined function must be ``@string::sTile``, ``@array::aList``, and
``@int::iIndex``. ``sTile`` and ``aList`` are the same arrays passed to ``GMFInsertArrayList``,
while ``iIndex`` is the **current** index.

The following is an example of simply rendering the text of each array item for the menu:

::

   scn SetListText
   string_var sTile
   array_var aList
   int iIndex
   string_var sText
   begin function {sTile, aList, iIndex}
	   Let sText := aList[iIndex]
	   GMFSetMenuStringValue ("%z\%.0f" sTile iIndex) "user1" sText
   end

Notice that we use ``iIndex`` for both the array indexing and for setting the string value. This is
why this argument is needed.
