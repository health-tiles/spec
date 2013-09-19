Health Tiles
============
Putting patient generated data at the fingertips of medical professionals.

_This is a working draft. File issues. Make pull requests._

**Objectives**
- Visualize data from patient sources (devices to applications)
- Integrate visualization within existing medical systems
- Provide hooks for pulling data from patient sources
- Reduce or minimize provider liability

**Technical Goals**
- Use existing standards to create implementation pattern
- Minimize effort on existing medical systems
- Needs to be secure

**Example Tiles**

![Glucose Example](assets/example1.png)
![Immunization Example](assets/example2.png)

**Physician Experience**

1. Pulls up a patient's record
2. Sees Health Tile(s) that a patient has added

For a physician or medical professional, seeing patient generated data is as easy as opening up the patient record.

**Patient Experience**

1. Logs into Patient Portal
2. Adds a tile by picking from a list (or entering a URL)
3. Authenticates with external service
4. Successfully adds tile

A patient only has to add the tile once, and it will be linked to their patient record.

Technical Details
============
For Health Tiles to be successful, they need to be **flexible** and **secure**. Flexible enough to allow patient-focused device and applications to express the data they are capturing in meaningful ways. Secure enough to enable these "tiles" to live in the user interface of an Electronic Health Record System.

An implementation pattern can be created using existing standards, specifically relying on HTML 5 and sandboxed IFRAMEs. The IFRAME provides patient-focused developers a canvas (with a fixed height and width) to populate.

![Health Tile](assets/dimensions.png)

Since IFRAMEs are being used, there is **no burden** on EHRs to process, store, or visualize data from patients. An EHR is only required to remember the URLs for each of the tiles and present them.

**Authentication**

(The set of steps for authentication need to be detailed. When a patient wants to add a Health Tile they will need to authorize their EHR to access to access their data. The most likely pattern is OAuth, but it can also be made simpler.)

**Security**

The ```sandbox``` ability of IFRAMEs allows us to apply security rules and grant certain privileges to the content being displayed.

**URLs**

The authentication step will most likely give the EHR a "token" to use to retrieve the particular tile for the patient. It may look something like this:

```https://some-health-tracker.org/[TOKEN]/tile```

This also enables an EHR to retrieve a machine-readable blob with structured information:

```https://some-health-tracker.org/[TOKEN]/json```

**Example**

For example, if a patient is using a digital insulin pump to manage their type I diabetes, they can add a a tile that displays their glucose readings over a period of time.

![Glucose Example](assets/example1.png)

The EHR would display the above by populating a sandboxed IFRAME with:

```https://digital-insulin-pump.org/[TOKEN]/tile```

If the EHR wanted to pull in any of the data, it could by retrieving the following URL:

```https://digital-insulin-pump.org/[TOKEN]/json```

Which could look something like:

```
{
  "device": "Digital Insulin Pump",
  "manufacturer": "ABC Corp.",
  "readings": [
    {"date": 20130915, "value": 100},
    {"date": 20130914, "value": 140}
  ]
}
```
