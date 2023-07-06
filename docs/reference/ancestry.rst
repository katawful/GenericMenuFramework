Ancestry Manipulation
=====================

It can be useful to walk through a tile's ancestry. While there is no native way to do this with
xOBSE or MenuQue, GenericMenuFramework provides this functionality

GMFGrabParentFloatTraits
------------------------
:Args: ``@string::sTile``, ``@string::sTrait``
:Info: Walks through a tile's ancestry, grabbing each float trait ``sTrait`` and putting it into an
	   array for return.
:Return: ``@array::aTraits``

GMFGrabParentStringTraits
-------------------------
:Args: ``@string::sTile``, ``@string::sTrait``
:Info: Walks through a tile's ancestry, grabbing each string trait ``sTrait`` and putting it into an
	   array for return.
:Return: ``@array::aTraits``
