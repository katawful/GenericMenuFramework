.. _valuefunctions:

Value Functions
===============

These functions either set or get values from menu tiles. They are predominantly wrapper functions
for existing xOBSE and MenuQue functions due to inherent limitations in the function signatures.

Setters
-------

Setters set values:

GMFSetTileFloatValue
____________________
:Args: ``@string::sTile``, ``@string::sTrait``, ``@float::fValue``
:Info: Set the float value of a trait for some tile, wrapper for ``GetMenuFloatValue`` from xOBSE

GMFSetTileStringValue
_____________________
:Args: ``@string::sTile``, ``@string::sTrait``, ``@string::sValue``
:Info: Set the string value of a trait for some tile, wrapper for ``GetMenuStringValue`` from xOBSE


Getters
-------

Getters get values:

GMFGetTileFloatValue
____________________
:Args: ``@string::sTile``, ``@string::sTrait``,
:Info: Get the float value of a trait for some tile, wrapper for ``GetMenuFloatValue`` from xOBSE
:Return: ``@float::fValue``

GMFGetByMenuTileFloatValue
__________________________
:Args: ``@string::sTile``, ``@string::sTrait``, ``@int::iMenuType``
:Info: Get the float value of a trait for some tile and menu type, wrapper for ``GetMenuFloatValue``
	   from xOBSE
:Return: ``@float::fValue``

GMFGetTileStringValue
_____________________
:Args: ``@string::sTile``, ``@string::sTrait``
:Info: Get the string value of a trait for some tile, wrapper for ``GetMenuStringValue`` from xOBSE
:Return: ``@string::sValue``

GMFGetByMenuTileStringValue
___________________________
:Args: ``@string::sTile``, ``@string::sTrait``, ``@int::iMenuType``
:Info: Get the string value of a trait for some tile and menu type, wrapper for
	   ``GetMenuStringValue`` from xOBSE
:Return: ``@string::sValue``

GMFGetTileName
______________
:Args: ``@string::sTile``
:Info: Get the immediate name of a tile, wrapper for ``tile_GetName`` from MenuQue
:Return: ``@string::sName``

GMFGetByMenuTileName
____________________
:Args: ``@string::sTile``, ``@int::iMenuType``
:Info: Get the immediate name of a tile of a menu type, wrapper for ``tile_GetName`` from MenuQue
:Return: ``@string::sName``

GMFGetTileFullName
__________________
:Args: ``@string::sTile``
:Info: Get the full name of a tile (i.e. ancestry of the tile), wrapper for ``tile_GetName`` from
	   MenuQue with the boolean option enabled
:Return: ``@string::sName``

GMFGetByMenuTileFullName
________________________
:Args: ``@string::sTile``, ``@int::iMenuType``
:Info: Get the full name of a tile (i.e. ancestry of the tile) of a menu type, wrapper for
	   ``tile_GetName`` from MenuQue with the boolean option enabled
:Return: ``@string::sName``
