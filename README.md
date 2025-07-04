# phonenumbers 
[![Build Status](https://github.com/madekrishna/phonenumbers/workflows/CI/badge.svg)](https://github.com/madekrishna/phonenumbers/actions?query=workflow%3ACI) 
[![codecov](https://codecov.io/gh/madekrishna/phonenumbers/branch/main/graph/badge.svg)](https://codecov.io/gh/madekrishna/phonenumbers)
[![GoDoc](https://godoc.org/github.com/madekrishna/phonenumbers?status.svg)](https://godoc.org/github.com/madekrishna/phonenumbers)

golang port of Google's libphonenumber, forked from [libphonenumber from ttacon](https://github.com/ttacon/libphonenumber) which in turn is a port of the original [Java library](https://github.com/googlei18n/libphonenumber/tree/master/java/libphonenumber/src/com/google/i18n/phonenumbers).

You can see a live demo of the number parsing of the master branch of this library at [https://phonenumbers.temba.io/](https://phonenumbers.temba.io) Compare results with the official [Google Java version](https://rawgit.com/googlei18n/libphonenumber/master/javascript/i18n/phonenumbers/demo-compiled.html).

This fork fixes quite a few bugs and more closely follows the official Java implementation. It also adds the `buildmetadata` cmd to allow for rebuilding the metadata protocol buffers, country code to region maps and timezone prefix maps. We keep this library up to date with the upstream Google repo as metadata changes take place, usually no more than a few days behind official Google releases.

This library is used daily in production for parsing and validation of numbers across the world, so is well maintained. Please open an issue if you encounter any problems, we'll do our best to address them.

# Version Numbers

As we don't want to bump our major semantic version number in step with the upstream library, we use independent version numbers than the Google libphonenumber repo. The release notes will mention what version of the metadata a release was built against.

# Usage

```go
// parse our phone number
num, err := phonenumbers.Parse("6502530000", "US")

// format it using national format
formattedNum := phonenumbers.Format(num, phonenumbers.NATIONAL)
```

# Rebuilding Metadata and Maps

The `buildmetadata` command will fetch the latest XML file from the official Google repo and rebuild the go source files 
containing all the territory metadata, timezone and region maps.

It will rebuild the following files:

 * `gen/metadata_bin.go` - protocol buffer definitions for all the various formats across countries etc..
 * `gen/shortnumber_metadata_bin.go` - protocol buffer definitions for ShortNumberMetadata.xml
 * `gen/countrycode_to_region_bin.go` - information needed to map a contry code to a region
 * `gen/prefix_to_carrier_bin.go` - information needed to map a phone number prefix to a carrier
 * `gen/prefix_to_geocoding_bin.go` - information needed to map a phone number prefix to a city or region
 * `gen/prefix_to_timezone_bin.go` - information needed to map a phone number prefix to a city or region

```bash
% go install github.com/madekrishna/phonenumbers/cmd/buildmetadata
% $GOPATH/bin/buildmetadata
```
