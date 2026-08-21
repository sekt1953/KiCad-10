# How to model a custom 3D part in FreeCAD

## Design 3d veiw in freecad for kicad 10.0

* To seamlessly design and bridge 3D views and models between FreeCAD and KiCad 10, the definitive industry standard is to use the KiCad StepUp Workbench. This allows you to create mechanical models in FreeCAD that perfectly align with your KiCad component footprints, board edges, and enclosures.
* The step-by-step workflow outlines how to model a custom 3D part in FreeCAD and display it natively inside KiCad 10's 3D Viewer:

1. Install the KiCad StepUp Workbench in FreeCAD
   1. Launch FreeCAD.
   2. Navigate to Tools > Addon Manager.
   3. Search for KiCadStepUp and click Install.
   4. Restart FreeCAD to load the new workbench.
2. Design the 3D Component Model
   1. Switch to the Part Design Workbench or Part Workbench.
   2. Create a new body and draw a sketch to outline your custom component (e.g., a connector, an IC, or a mechanical switch).
   3. Extrude the sketch using Pad to turn it into a 3D solid.
   4. Optional: If your model has multiple colored segments (like metal pins vs. a plastic housing), use a Boolean Fuse/Union or construct a unified Compound Object. KiCad StepUp requires a unified gizmo or combined structure to cleanly track materials.
3. Align and Export Using StepUp
   1. Load your footprint file (.kicad_mod) directly into FreeCAD using the StepUp toolbar to serve as a placement guide.
   2. Rotate and align your custom 3D model until it sits perfectly on the pins and pads of the imported KiCad footprint.
   3. Select your completed, aligned 3D model hierarchy in the FreeCAD tree.
   4. Click the Export 3D Model to KiCad tool on the StepUp toolbar.
   5. StepUp will generate two files simultaneously:
     * A .step file (used for accurate mechanical CAD and enclosure collision checking).
     * A .wrl (VRML) file (used for realistic, colorful rendering in KiCad's built-in 3D viewer).
4. Link and Preview in KiCad 10
   1. Open your PCB layout layout (.kicad_pcb) or the Footprint Editor in KiCad 10.
   2. Double-click the target footprint to open its properties, then click the 3D Models tab.
   3. Click the folder icon, browse to your project directory, and select your exported .wrl or .step file.
   4. Because you already aligned it perfectly using StepUp in FreeCAD, the model should load with zero coordinate offsets (X:0, Y:0, Z:0).
   5. Click OK, then press Alt + 3 (or navigate to View > 3D Viewer) to visualize your populated board in full 3D.

## design a physical enclosure

*  If you are instead looking to design a physical enclosure around an entire completed board layout, what specific enclosure style or mounting constraints does your project require?