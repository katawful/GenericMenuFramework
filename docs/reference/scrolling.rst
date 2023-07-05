Scrolling a List
================

While scrolling a list is one of the more complex features of menus, this framework makes this very
simple. Assuming you followed the advice in :ref:`List Generation <ListGeneration>`, for the list
itself adding scrolling to a menu becomes very simple.

Scroll Bar Tile
---------------

The scroll bars are generally provided by menus themselves via prefabs, for example:

::

   <image name="scroll_bar">
	   <include src="vertical_scroll.xml"/>
	   <depth> 4 </depth>
	   <id> 112 </id>
	   <user1> 0 </user1>
	   <user2>
		   <copy src="list_rect" trait="child_count" />
		   <sub src="me()" trait="user8" />
		   <add> 2 </add>
	   </user2>
	   <user3> 1 </user3>
	   <user4> 6 </user4>
	   <user5> 0 </user5>
	   <user6> 33 </user6>
	   <user8> 8 </user8>
   </image>

The user traits are integral to operating a scroll bar:

:user1: Minimum amount of items to view
:user2: Maximum scroll amount. This trait should be set to the child count of the list ``rect``.
	    It's pretty important to be specific with your own menu
:user3: Amount scroll bar moves when arrow of scroll bar is clicked
:user4: Amount scroll bar moves when bar of scroll bar is clicked
:user5: The scroll bar's writable current position
:user6: The ID of the scroll bar's marker
:user7: The scroll bar's readable current position
:user8: Number of items visible

GMFOnScrollVerticalInitFunction
-------------------------------
:Args: ``@string::sTile``, ``@array::aIDs``
:Info: This function is intended to be called continuously, such as in a ``begin menumode 1011``
	   loop in a quest script, to provide scrolling controls. ``sTile`` is the scroll bar tile, and
	   ``aIDs`` is an array of numeric IDs that you wish this scrolling to affect.

	   This function is for a vertical scroll bar.

GMFOnScrollHorizontalInitFunction
-------------------------------
:Args: ``@string::sTile``, ``@array::aIDs``
:Info: This function is intended to be called continuously, such as in a ``begin menumode 1011``
	   loop in a quest script, to provide scrolling controls. ``sTile`` is the scroll bar tile, and
	   ``aIDs`` is an array of numeric IDs that you wish this scrolling to affect.

	   This function is for a horizontal scroll bar.

GMFOnDragInitFunction
---------------------
:Args: ``@string::sTile``
:Info: This function initializes mouse dragging of a scroll bar. Like the two scroll functions,
	   ``sTile`` takes the scroll bar you wish to scroll. Unlike the 2, this function works on
	   either vertical or horizontal scroll bars. This function needs to be called continuously as
	   well.
