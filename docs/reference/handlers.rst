Event Handlers
==============

Event handlers are functions that only run when a certain game event runs. They are useful when one
wants to handle many different states at once. All event handler functions in this framework are
wrappers around MenuQue event handlers.

Generic Menu Handlers
---------------------
These functions are handlers for generic menus and for any tile ID.

GMFSetOnClickHandler
____________________

:Args: ``@ref::rFunction``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On left click of mouse, for any tile ID

GMFSetOnOpenHandler
___________________
:Args: ``@ref::rFunction``
:Handler Args: ``@int::iMenuType``
:Info: On open of a generic menu, for any tile ID. Note that this function will run before
	   most of the menu is done rendering

GMFSetOnCloseHandler
____________________
:Args: ``@ref::rFunction``
:Handler Args: ``@int::iMenuType``
:Info: On close of a generic menu, for any tile ID

GMFSetOnMouseoverHandler
________________________
:Args: ``@ref::rFunction``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On mouseover of a tile, for any tile ID


Generic Menu Handlers with ID
-----------------------------
These functions are handlers for generic menus with specified tile IDs.

GMFSetOnClickByIdHandler
________________________
:Args: ``@ref::rFunction``, ``@int::iID``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On left click of mouse, for a specific tile ID

GMFSetOnOpenByIdHandler
_______________________
:Args: ``@ref::rFunction``, ``@int::iID``
:Handler Args: ``@int::iMenuType``
:Info: On open of a generic menu, for a specific tile ID. Note that this function will run
	   before most of the menu is done rendering

GMFSetOnCloseByIdHandler
________________________
:Args: ``@ref::rFunction``, ``@int::iID``
:Handler Args: ``@int::iMenuType``
:Info: On close of a generic menu, for a specific tile ID

GMFSetOnMouseoverByIdHandler
____________________________
:Args: ``@ref::rFunction``, ``@int::iID`` , ``@int::iID``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On mouseover of a tile, for a specific tile ID


Any Menu Handlers
-----------------
These functions are handlers for any menu type and for any tile ID.

GMFSetByMenuOnClickHandler
__________________________
:Args: ``@ref::rFunction``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On left click of mouse on a tile in a specific menu type, for any tile ID

GMFSetByMenuOnOpenHandler
_________________________
:Args: ``@ref::rFunction``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``
:Info: On open of a specific menu type, for any tile ID. Note that this function will run
	   before most of the menu is done rendering

GMFSetByMenuOnCloseHandler
__________________________
:Args: ``@ref::rFunction``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``
:Info: On close of a specific menu type, for any tile ID

GMFSetByMenuOnMouseoverHandler
______________________________
:Args: ``@ref::rFunction``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On mouseover of a tile in a specific menu type, for any tile ID


Any Menu Handlers with ID
-------------------------
These functions are handlers for any menu type with specified tile IDs.

GMFSetByMenuOnClickByIdHandler
______________________________
:Args: ``@ref::rFunction``, ``@int::iID``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On left click of mouse on a tile in a specific menu type, for a specific tile ID

GMFSetByMenuOnOpenByIdHandler
_____________________________
:Args: ``@ref::rFunction``, ``@int::iID``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``
:Info: On open of a specific menu type, for a specific tile ID. Note that this function will
	   run before most of the menu is done rendering

GMFSetByMenuOnCloseByIdHandler
______________________________
:Args: ``@ref::rFunction``, ``@int::iID``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``
:Info: On close of a specific menu type, for a specific tile ID

GMFSetByMenuOnMouseoverByIdHandler
__________________________________
:Args: ``@ref::rFunction``, ``@int::iID``, ``@int::iMenuType``
:Handler Args: ``@int::iMenuType``, ``@string::sTile``, ``@int::iID``
:Info: On mouseover of a tile in a specific menu type, for a specific tile ID

