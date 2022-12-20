## Shared Responsibility Model
The shared responsibility model is a model that AWS adopts to ensure data security for all AWS customers. The whole idea behind this model is to divide the responisiblity for data security into two distinct parts, in which AWS would be solely in charge on ensuring the security for the base systems and services of AWS, whilst the customer would be in charge on ensuring the security of their own data.

![[Pasted image 20221220110952.png]]

## AWS Responsibility
| Category | Examples of AWS Services in the Category | Responsibility |
| - | - | - |
| Infrastructure Services | Compute services, such as EC2 | Manages underlying infrastructure and foundation services |
| Container Services | Services that require less management from the customer, such as RDS | Manages the underlying infrastructure and foudnation services, operating system, and application platform |
| Abstracted Services | Services that require minimal management from the custormer, such as S3 | Operates infrastructure layer, operating system, platforms, server-side encryption, and data protection |

## Customer Responsibility
| Category | Responsibility |
| - | - |
| Infrastructure Services | Operating system, application platform, encrypting, protecting, and managing customer data |
| Container Services | Encrypting, protecting, and managing customer data through network firewalls and backups |
| Abstracted Services | Protecting and managing customer data through client-side encryption |