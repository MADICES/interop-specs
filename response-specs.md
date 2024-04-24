In this document we are design a common shared format standard to exchange data between different platforms (say from P2 to P1) that include both data and metadata. The focus is to enable the ability to exchange the subset of all metadata available in P1 and that is ontologized, in such a way that platform P2 can understand it (and potentially import it), as long as it knows/understands the same concepts.

<b>Note</b>: In general, platform P2 will be able to export a complete (custom format) file that will contain both data and metadata (e.g. internal metadata needed to know in which DB table the data is stored, the location of raw files, and any additional information needed to import the data back in the same platform P2). For the purpose of this exchange/response format, this should be considered as "data" (and not metadata). P1 can store it as a raw file without the need of understanding it (while P2 could use it to import back, if needed/wanted).

However, for any concept that is ontologized, those would become part of what we call here metadata, and are the parts of information that can (potentially, not necessarily) be undertsood by another platform (in this example P1).


<b>Additional comment</b>: we will work for now with the assumption that the format that we are designign can serve 2 goals:
 - a response to a request (e.g. give me all "STMImages" you have, where "STMImages" is a URI to an ontological concept)
 - an "export file" that P2 generates, e.g. because they think that those "STMImages" are very relevant. The export file (whose format is described here) can be e.g. put on Zenodo, and other platforms (e.g. P1) can decide to get the file and interpret it (possibly importing it).

The ultimate goal could be:
- P2 exports some set of data, including some specific datasets with ontological annotations
- P1 can get the data, store it raw, but also find the list of ontologized metadata
  - P2 can then check if it understands those concepts, and import them appropriately (if it makes sense). E.g. if P1 exports an ontologized STMImage, and P2 understands it, it can store it as such, and this would enable for instance the visualization of the image with an appropriate viewer, the display of sliders e.g. to select the current, etc.

In addition, it would be important to also have the possibility to express relations (probably via hasParts?). So I can export a crate with 3 files:
- a raw file that allows me to re-import the data , treated just as a file
- an ontologized STMImage IM1, with ontologized metadata, and a link to a folder in the RO-crate so that an appropriate STM Image parser can parse it
- an ontologized crystal structure S1 (or at least a link to a Unique ID of the structure in a DB), that represents the structure for which the STM image was computed or measured
and I want to add explicitly a link that says "this image S1 comes from this crystal structure IM1"
In this way:
  - a platform that does not understand structures will both ignore S1 and the link
  - a platform that understands boths, will import both S1 and IM1, but ignore the link possibly (maybe just showing to the user the information "I know that IM1 is related to S1" and S1 is just a string with the unique ID)
  - a platform that cares about relationships would also store it (e.g. to show it visually when looking to all calculations and experiments related to structure S1)





Should return RO-Crate file format (.eln ?):

```
<RO-Crate root directory>/
    - ro-crate-metadata.json               # RO-Crate Metadata File MUST be present 
    - platform-data-X.any                  # 1 or more
    - ... additional files and folders     # 0 or more
```

<b>platform-data-X.any</b> if present, should contain the raw objects of the target platform as the are queried in the original platform, in whatever native format the platform generate them.

```python
class MinimalObject:
    id: str
    type: str
```
where <strong>id</strong> is the unique identifier in the plaftorm where the objects belongs and <strong>type</strong> is an URI pointing to a generic ELN Object (we need to define) or a specific subclass.

```json
[
    {
        id:"20240424084127547-750",
        type:"http://openbis.ethz.ch/uri/as.dto.dataset.DataSet"
    },
    ...
]
```
- the type is or not an Ontology ? 
- how do we know and recognize what the type means in the specific platform ? 
- should add a field ?



