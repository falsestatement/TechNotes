## Availabilitity Zones
Groups of AWS data centers that are acting as backups for one another all in close proximity. This prevents down time when one data center goes offline. Avaiablility zones also have an availability zone code that looks like a [[##Regions|region]] code appended by a letter, eg. `us-east-1a`.

## Regions
Regions are groups of [[#Availabilitity Zones|AZ]]s, which help to create redundant systems to prevent down time even when an entire [[#Availabilitity Zones|AZ]] happens to go down. Regions are also usually named after their physical geographical location, and have region codes that looks like `us-east-1`.

## Region Considerations
- Compliance
	- Check for business related compliances issues
	- Business specific [[##Regions|region]] specifications
- Latency
	- Closer [[##Regions|regions]] to the business have lower latency
	- Consider user base locations
- Pricing
	- Not all [[##Regions|regions]] have the same pricing structure
	- Uses local laws as guidance for pricing
	- Some [[##Regions|regions]] may be cheaper than others
- Service Availability
	- Not all [[##Regions|regions]] have the same features / services
	- Newer services are rolled out incrementally, not immediate across all [[##Regions|regions]]

## Scope
Some AWS services can be scope specific, meaning that some services may want you to declare a specific [[##Regions|region]] and / or a specific [[#Availabilitity Zones|availability zone]]. Region-scoped services are services that would automatically perform actions to increase data durability and availability. AZ-scoped services, on the other hand, usually require the user to maintain data durability and availability.

## Maintaining Resiliency
It is usually best practice to use [[##Scope|region-scoped]] services, as they have availability and resiliency built in. However, in the circumstance that does not allow the use of a [[##Scope|region-scoped]] service, the user must make sure the same settings are applied to multiple [[#Availabilitity Zones|AZ]]s at once to ensure there is no mismatch or down time when any specific [[#Availabilitity Zones|AZ]] goes offline.