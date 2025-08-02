# immich-migrate-people
Migrate named people in images between immich accounts

**!!! This has only been tested on Ubuntu OSes, but should work on other linux based systems !!!**

## Overview
Exports people names / faces associated with images from one immich account to another account. This can be between two accounts on the same instance or seperate instances. Matching between accounts is done on asset name. I did not have any issues with duplicate asset names as the face match is based on the position (the square) of the face detected.

Please note that faces which are *unassigned* will not be matched.

You will need to manually merge people.  Duplicates and the number of duplicate names are listed at the end of the output.

## Requirements:
- Each immich account requires an api key
   - Navigate to your Account settings
   - Expand **API Keys** section
   - Click **New API Key** button
   - Provide a name and click **Create**
   - Click **Copy to Clipboard**
   - Save to a secure location
   - Click **Done**
## Installing
- Download the tar.gz file and uncompress
- Edit the configuration files setting your http://immich:2283 sever name and appropiate API keys
  - immich.export.config
  - immich.import.config


### immich.export.config
```ini
server=http://docker1:2283

# from@domain.tld
apiKey=QKLLxISmpuvJAzCjxQbCWg1FRPsuI8ndvMyti9sQ

```

### immich.import.config
```ini
server=http://docker1:2283

# to@domain.tld
apiKey=bIln15UFJwZrKJy9TeNK8BASO7t4IS9Fmof8YByd9s

```

## Executing
### Exporting (this will not modify your immich data)
```
./export.people
```

output:
~~~ getting all people info. This may take awhile ......
people retrieved 100
        .... done got 163.

assets processed 5642   for 163 / 163 people
~~~

### Importing
```
./import.people
./import.people -test
```

output:
```
-=-=- Might need to run 'Facial Recognition' job for missing assets before running this script -=-=-

Control-C to exit. Enter to continue.


getting all people info. This may take awhile ......
people retrieved 6700
        .... done got 6755.

[lines redacted]

line 50 / 6018  (0 assets)      1994_06_04_221434.png
updating person (76e25db1-c1e7-4c75-afe1-d16f6c85024d) with Jon,  from line 52)

updating person (d0767ca8-e7cd-4d28-b356-049dfeffb43b) with Jon,  from line 53)

updating person (07c19cc8-b449-4270-ba9d-57cd803fd0ef) with Jon,  from line 62)

updating person (6b2ca9d2-185f-487e-b40c-a48c35ae0203) with Jon,  from line 68)

updating person (1fba7845-4892-4d21-b0ab-6c6ef26b1250) with Jon,  from line 76)

updating person (0fe14125-ee55-4d60-998a-41266808d472) with Jon,  from line 80)
line 100 / 6018 (1127 assets)   slide_043.jpg
updating person (be36ee7d-2fcb-4360-b093-1040a63bef23) with Jane,  from line 158)
line 300 / 6018 (3517 assets)   1952_125524.png
updating person (474f3f19-0585-4bb9-af41-52bd72a192a9) with John,  from line 305)

updating person (f24b967a-f154-4452-a439-19f070461335) with Eric ,  from line 336)

updating person (e8cc9bf1-fd32-4cfa-85b9-720e3f8f227f) with Eric ,  from line 339)
line 350 / 6018 (4110 assets)   slide_025.jpg
updating person (c9083e70-05c4-4c74-a4cb-d5b0277af085) with Eric ,  from line 353)

updating person (0051f096-1b17-4ad9-8923-9f9feddae365) with Eric ,  from line 355)

updating person (ac2fcbca-0be0-48ef-acef-3b53b2b88a64) with Eric ,  from line 356)

[many lines redacted]

line 6018 / 6018        (11073 assets)

***
**
looking for duplicate names. (This too may take awhile) ......
people retrieved 6700
        .... done got 6755.



duplicate names:
  B (4)
  A (8)
  G (2)
  S (13)
  M (6)
  J (4)
  R (19)
  B (15)
  D (2)
  T (10)
  A (3)
  E (20)


Done.
```


## Testing
You can edit the generated output **people.export** file reducing to test assets before comitting to a full run.
Additionally, executing ```./import.people -test``` will display what will change.

### How I tested.
1. I exported from a small (4000+) external library I have of scans from the in-laws.
2. I created a new test user on the same immich instance and created a new API key.
3. As admin, I created a new external library, owned by the new test user, a subset of that inital external library containing 899 images.
4. I scanned that new test library and waited for all jobs to complete.
5. All found users are un-named.
6. Ran the import script.
7. Spot checked the results.
   

