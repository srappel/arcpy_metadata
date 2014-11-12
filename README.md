Arcpy Metadata Editor (arcpy_metadata)
==============
Whether you create it or not, metadata is a critical part of GIS analysis. ArcGIS includes a built-in GUI metadata editor, but has scant access to metadata properties from Python. The arcpy_metadata package provides this access, allowing large Python packages that generate their own geospatial outputs in ArcGIS to properly document the data.

Getting arcpy_metadata
----------------------
arcpy_metadata is pure Python and its only dependency is arcpy (installed with ArcGIS). It's available on the Python Package Index so you can get arcpy_metadata via pip (pip install arcpy_metadata).
 
If you don't have or don't know how to use pip, you can install arcpy_metadata by cloning/downloading this repository and running setup.py install in the root folder

Using arcpy_metadata
--------------------
As of this writing, documentation is still to come, but the source code is short, so we encourage you to take a look at it in the meantime. Otherwise, following is an example usage of arcpy_metadata:

```python

import arcpy_metadata as md
import datetime

metadata = md.MetadataEditor(path_to_some_feature_class)  # also has a feature_layer parameter if you're working with one, but edits get saved back to the source feature class
metadata.title.set("The metadata title!")

generated_time = "This layer was generated on {0:s}".format(datetime.datetime.now().strftime("%m/%d/%Y %I:%M %p"))

metadata.purpose.set("Layer represents locations of the rare Snipe.")

metadata.abstract.append("generated by ___ software")
metadata.abstract.append(generated_time)  # .prepend also exists
metadata.tags.add(["foo", "bar", "baz"])  # tags.extend is equivalent to maintain list semantics

metadata.finish()  # save the metadata back to the original source feature class and cleanup. Without calling finish(), your edits are NOT saved!
```
The code is based on a set of core classes that provide set/append/prepend operations, and we would love pull requests that add classes or attributes to cover additional portions of metadata specs.

Under the hood
---------------
arcpy_metadata uses the strategy of exporting the metadata from the layer, then edits the xml export based on your method calls. When you're done, use finish() to save your data back to the source.

