# Cantrel SOA-C02 Notes

## AWS 

## IAM, Accounts and AWS Organizations
> Service Catalog
- Document or Database created by an IT team
  - Organized collection of products
  - Offered by the IT Team - Cross charge products to other teams
  - Key product information: Product owner, Cost, Requirements, Support Information, Dependencies
  - Defines approval of provisioning from IT and Customer side
  - Manage costs and scale service delivery
- Self-service portal for End-Users
  - Launch predefined (by admin) products
  - End user permissions can be controlled
- Admins can define these products and the permissions required to launch them
- The admin can build products into portfolios

> AWS Service Catalog
  1) Admin defines products and portfolios using CloudFormation Templates and Service Catalog Configurations
  2) Deploy portfolio to any service enabled regions
  3) Service catalog users review portfolios they have permissions on and launch products(s) into service enabled regions
  4) Service Catalog launches the infrastructyure using defined templates. Service catalog users don't need infrastructure permissions - Only launch permissions
  5) Products are available for use

> AWS Cost Explorer
- Cost Explorer - You can inquire into the costs associated with services, accounts, groups, etc.
- Tool that you can use to create and analyze usage and costs within the account
- You can also use to create recommendations to reduce costs to your account - ex. Recommending reserved instances, etc.

> Cost Allocation Tags
- Cost allocation tags - Have to be enabled individually (per account for standard accounts or ORG Master for ORGS)
    1) AWS Generated (ex. aws:createdBy or aws:cloudformation:stack-name)
    - Added to resources AFTER the are enabled by AWS
    2) User-defined tags can also be enabled (user:something)
        - Both are visible in cost reports and can be used as a filter
- Adding cost allocation tags can take up to 24h to be applied
- These are NOT retroactive

## EC2
> Service Linked Roles and PassRole
- Service Linked Roles - IAM Role linked to a specific AWS Service
  - Predefined by a service
  - Provides permissions that a service needs to interact with other AWS services on your behalf
  - Service might create/delete the role, or allow you to during the setup or within IAM
  - You cannot delete the role until it's no longer required (if service is in use, you cannot delete it)
- Role separation - Giving users different permissions to access or modify services
  - Roles can be created and passed onto user's who have different permissions. 
  - A method used in AWS to give permissions to users that allows role separation

> EC2 Purchase Options (Launch Types)
- Default - On Demand (No savings - Dedicated instances)
  - Isolated instances but multiple customer instances run on shared hardware
  - Per-second billing while instance is running. Associated resources such as storage consume capacity, so bill, regardless of instance state
  - Instances of different sizes run on the same EC2 host - consuming a different allocation of resources
  - Default Purchase Option
    - No interruption
    - No capacity reservations
    - Predictable pricing
    - No upfront Cost
    - No discount
    - Short term workloads
    - Unknown Workloads
    - Apps that cannot be interrupted
- Spot Pricing (Pay for spare room - Not dedicated instances)
  - Spot pricing is selling unused EC2 host capacity for up to 90% discount - spot price is based on the spare capacity at any given time
  - You set the price that you are willing to pay and if the current spot price is less than the set price, you will have the EC2 instance(s) allocated to you and you pay that price
  - When spot prices increase higher than your set price, the EC2 instance is unallocated from you and your workload ceases
  - Workloads need to be able to handle disruptions and workloads that are NOT time critical, stateless, bursty and cost sensitive
- Reserved Instances (Pay ahead for capacity for a saving - Dedicated Instances)
  - Long term consistent uses for EC2 - Reserves capacity for an EC2 instance
  - Purchasing a reservation allows you to reduce the per-second cost as you pay for this instance ahead of time - Planned instance use
  - If you purchase an instance and you do not use it, you still pay for the reservation.
  - You can have partial reservation savings (ie. if the reservation is smaller than what you configure, you will get a saving for the capacity up to the reservation and then pay the remaining at the on demand price)
  - Use term lengths from 1 to 3 year lengths
    - You can pay with a no-upfront where you pay a cost for a reservation and then pay a reduced price for the per/s fee
    - You can pay All upfront, so you are NOT paying a per/s fee
    - You can do a mix of the 2, which will typically give more savings the more you pay upfront
- Dedicated Hosts (Most admin - Entire Host is managed BY YOU)
  - You pay for the host - Any instance that runs on the host has NO per/s charge
  - If the host runs out of capacity, you cannot run more instances on the host
  - Since you "own" the host, you have access to the physical hardware and can allocate sockets/cores. This can impact licensing requirements
- Dedicated Instances (Access to entire host - Entire Host is managed BY AWS)
  - You pay AWS for access to a dedicated host but you don't own or share the host. This incurs extra cost for the instance, as you have dedicated hardware on the host that AWS manages for you. 
  - You pay a per/h cost for a dedicated host and also for the instances itself.
  - No managing of capacity - AWS manages the host for a premium 

> Reserved Instances (More Options)
- Scheduled Reserved Instances
  - Ideal for long term usage which DOES NOT run constantly
  - Cheaper
    - Ex. You pay for reservations for scheduled times only 
    - You purchase time blocks
    - Requires 1 year commitment 
    - Minimum 1200 Hours per year
    - Not available for all instance types
  - Used for EC2 instances that run in batches or in times that do not require uptime 24/7
- Capacity Reservations - It should be noted that AWS allocate resources in this order: 1) To reserved instances -> 2) To on demand instances
  - Different from reserved instances
    - You may require capacity for EC2 instances but you cannot commit to reserving an instance
    - Noes not require a 1-3 year commitment
  - Regional Reservations - Provide a billing discount for valid instances launched in ANY AZ in THAT REGION
    - While flexible, they don't reserve capacity within an AZ - Risky during major faults when capacity can be limited
  - Zonal Reservations - Only apply to ONE AZ providing billing discounts and capacity reservation in THAT AZ ONLY
  - Option of Full Price & No Capacity Reservations
  - On-Demand capacity reservations can be booked to ensure you always have access to capacity in an AZ when you need it, but at FULL ON-DEMAND price. No term limits - you pay regardless of if you consume it
- EC2 Savings Plan 
  - Making an hourly commitment for 1-3 year term
  - In exchange for the commitment, you get a saving for the on-demand price
    - A general compute $ amount (ex. $20 per hour for 3 years)
    - Or a specific EC2 savings plan - flexibility on size and OS
  - Compute products, currently EC2, Fargate and Lambda
  - Products have an on-demand rate and a savings plan rate
  - Resource usage consumes saving plan commitment at the reduced saving plan rate
  - Beyond your commitment, you are charged On-Demand price