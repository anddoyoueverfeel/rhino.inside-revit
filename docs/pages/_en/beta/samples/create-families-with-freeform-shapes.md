---
title: Creating Families in Revit with Grasshopper
order: 8
---

{% include ltr/en/wip_note.html %}

This sample shows how to create a new Revit family in with Rhino geometry. In this case, a column is created and placed along a curve.

There are a lot of ways to bring in the Rhino geometry, but in this case we will be bringing them in as Family Type instances. This has a big advantage over other methods.  If the Freeform geometry is wrapped within family then it has 3 advantages:

1. The Graphic properties can be edited including the hatching and section line.  This is a big advantage over directshapes.
2. The geometry can be edited directly in the Family editor, within reason.
3. The instances will schedule properly.

![Rhino to Revit as a Family]({{ "/static/images/column_family_final.jpg" | prepend: site.baseurl }})

## Bringing Breps into a Revit Family
Rhino surfaces and brep solids can be imported into Revit as part of a Loadable Component Family.

Open Sample files:
1. Open the a new default project in Revit.
1. Start Rhino.Inside.Revit and open the [Column_on_Curve.3dm]({{ "/Column_on_Curve.3dm" | prepend: site.baseurl }}) file.
1. Start Grasshopper within Rhino and open the [Column_Family_Rotation.gh]({{ "/Column_Family_Rotation.gh" | prepend: site.baseurl }}) file.

The column should populate the points along the curve in both Rhino and Revit.

## Creating a Family definition

The inputs needed to make a Family definition are a point and the BREP geometry. In this case the first point on the curve serves as the point under the column assembly.  The column is setup with the foundation, column and small shelf shape.

### Moving Geometry to 0,0,0

The key to making a new family and being able to insert it with accuracy is to move the Rhino BREPs to the world origin.  Revit will always make new Families centered at the Origin (0,0,0).  Here the first point in the list is taken out of the list of points, a vector is made from that point to 0,0,0 and then that transform is applied to the Breps.  This places the geometry at 0,0,0 in Rhino temporarily.

![Move Rhino Geometry to 0,0,0]({{ "/static/images/column_family_move.jpg" | prepend: site.baseurl }})

Next the New Family component is used.  There are a series of inputs to the New Family component:

1. Specify a template file (.rft) for the family. In this case the standard Column template is used.  A ty[ical location for the templates is C:\ProgramData\Autodesk\RVT 2019\Family Templates\English_I\Column.rft.  Templates set many of the default parameters on new Families.
1. The Overwrite input allows Grasshopper to overwrite the family definition if set to true.  This can be used to trun off and on Grasshoppers ability to change Families.
1. The next input is the name of the resulting Family.
1. The Category that the Family belongs in is next as input.
1. Lastly the transformed Rhino Gemoetry at 0,0,0 is input.

![Create Revit Family]({{ "/static/images/column_family_family.jpg" | prepend: site.baseurl }})

Once the Family is created, it may be inserted.

## Inserting a Family Instances

Technically, Families cannot be inserted in Revit, only Types of a Family may be inserted.  To insert a Teyp Instance, use the AddFamilyInstance.ByLocation component:

![Get Type]({{ "/static/images/samples/new-family04.png" | prepend: site.baseurl }})  

The Add Family Instance component requires:

1. A Type to insert
2. A point or plane for location
3. A Level to host the object on (optional)

So the first step is to convert the Family to a Type. Because component Families only have one Type, it is quite easy to add a GetType component to get to the Type.

![Get Type]({{ "/static/images/samples/new-family03.png" | prepend: site.baseurl }}){: style="width:450px; "}

To place a component a simple point can be used. although this does not allow for initial rotational control. Using a Plane for the location not only is the insertion point controlled, but also the orientation/rotation also controlled. 

Hopefully this guide helps understand the importance the the steps necessary of creating new Families for Rhino geometry in Revit.

