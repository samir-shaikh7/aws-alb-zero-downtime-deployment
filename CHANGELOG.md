# Changelog

All notable changes to this project will be documented in this file.


---

## [1.0.0] - Initial Deployment

### Added

- Launched EC2 Instance **Server 1**
- Launched EC2 Instance **Server 2**
- Deployed Website Version 1
- Configured HTTP (80) and HTTPS (443) inbound rules
- Created Target Group **TG-Web-V1**
- Registered Server 1 and Server 2
- Created Application Load Balancer (**MyALB**)
- Associated TG-Web-V1 with the ALB
- Verified website using the ALB DNS name

---

## [2.0.0] - Zero Downtime Deployment

### Added

- Launched EC2 Instance **Server 3**
- Launched EC2 Instance **Server 4**
- Deployed Website Version 2
- Created Target Group **TG-Web-V2**
- Registered Server 3 and Server 4

### Changed

- Updated ALB Listener Rule
- Added TG-Web-V2 to the forwarding rule
- Configured weighted routing:
  - TG-Web-V1 → Weight: 0
  - TG-Web-V2 → Weight: 100

### Result

- Website upgraded successfully
- Same ALB DNS URL maintained
- Zero downtime achieved
- Traffic shifted completely to Website Version 2