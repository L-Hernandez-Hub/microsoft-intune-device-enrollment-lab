# Microsoft Intune Device Compliance & Conditional Access Troubleshooting Lab

## Scenario
A user can sign into Microsoft 365, but the organization wants access allowed only from compliant managed Windows devices. After the device is enrolled and secured, a compliance failure is introduced. Conditional Access should then block access until the device is remediated.

---

## Lab Environment 
- Microsoft Intune admin center
- Microsoft Entra admin center
- Windows 11 Virtual Machine

---

## Steps

### 1. Device Enrollment into Microsoft Intune
Created a security group titled `Intune-Windows-Users` in Microsoft Intune and added a licensed test user, `Ava Patel`. Enrolled a Windows 11 device into Microsoft Intune by connecting the user account through “Access work or school” and joining the device to Microsoft Entra ID. Verified successful enrollment by confirming the device appeared in both Microsoft Entra ID and Intune with compliant status.

![Screenshot 1](Screenshots/1-intune-confrimation.png)

![Screenshot 2](Screenshots/2-entra-confirmation.png)

![Screenshot 3](Screenshots/3-confirmation-test-machine.png)

---

### 2. Applied a Windows Configuration Profile
Deployed a Windows configuration profile using Intune’s Settings Catalog to enforce baseline security settings on enrolled devices. Configured settings such as sign-in behavior on resume and device sleep timeout to align with organizational security standards. Assigned the profile to the group and verified successful deployment by confirming policy status as Succeeded on the target device.

![Screenshot 4](Screenshots/4-settings-catalog.jpeg)

![Screenshot 5](Screenshots/5-device-status-success.jpeg)

![Screenshot 6](Screenshots/6-group-assignment.jpeg)

---

### 3. Created a Device Compliance Policy
Created a Windows device compliance policy in Microsoft Intune to enforce security standards required for device trust. Configured requirements including BitLocker encryption, Firewall, Antivirus, Antispyware, Microsoft Defender Antimalware, and real-time protection. Assigned the policy to the group and validated compliance by confirming the enrolled device reported as Compliant, preparing it for Conditional Access

![Screenshot 7](Screenshots/7-compliance-settings.png)

![Screenshot 8](Screenshots/8-group-assignment.jpeg)

![Screenshot 9](Screenshots/9-compliant-success.png)

![Screenshot 9.5](Screenshots/9.5-device-secruity-compliant.png)

---

### 4. Created a Conditional Access Policy in Report-Only Mode and Enforced Conditional Access Based on Device Compliance
Created a Conditional Access policy in Microsoft Entra ID to require compliant devices for accessing Office 365 resources. Configured the policy to target the group and applied it to Office 365 cloud apps. Set the access control to `Require compliant device` and initially enabled the policy in `Report-only mode` to safely evaluate its impact.

Performed test sign-ins using the enrolled user account and reviewed sign-in logs to confirm the policy was triggered and evaluated successfully. After validation, switched the policy from Report-only to `On` to enforce access control based on device compliance.

![Screenshot 10](Screenshots/10-conditional-access-policy.png)

![Screenshot 11](Screenshots/11-report-only-success.png)

![Screenshot 12](Screenshots/12-enforcement-on.png)

---

### 4. Simulated a Noncompliant Device Condition
Intentionally modified the device by disabling BitLocker encryption to simulate a non-compliant endpoint. After forcing a device sync, the device was marked as Noncompliant in Microsoft Intune due to failing the BitLocker requirement defined in the compliance policy created. Verified non-compliance at the policy level, confirming BitLocker as the root cause. Attempted to access Microsoft 365 resources, where Conditional Access enforcement blocked access and required the device to meet compliance requirements.

![Screenshot 13](Screenshots/13-device-noncompliant.png)

![Screenshot 14](Screenshots/14-windows-security-compliance-noncompliant.png)

![Screenshot 15](Screenshots/15-policy-settings-status.png)

![Screenshot 16](Screenshots/16-access-blocked-screen.png)
