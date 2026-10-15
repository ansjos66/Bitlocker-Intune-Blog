# Lost Your Laptop? How BitLocker and Intune Keep Your Business Data Safe
A step-by-step guide to encrypting every Windows 11 device in your business with Microsoft 365
The laptop you left in the park
Your laptop gets stolen, or you simply leave it in a park. It is full of confidential company data. Should you be worried? You should, if that drive isn’t encrypted. Whoever finds the laptop doesn’t need your Windows password. They can pull out the hard drive, plug it into another machine and read everything on it. Imagine a sales manager at a small accounting firm. She stops for coffee on the way to a client meeting and leaves her laptop bag on the back seat of her car. Twenty minutes later, the window is smashed and the bag is gone. The laptop itself is easy to replace. What’s on it is not. It holds client tax returns, payroll spreadsheets and bank details for dozens of customers. Now the firm has to tell every one of those clients their data may have been exposed. It may also have to report the breach to the authorities and deal with the damage to its reputation. If that laptop had been encrypted with BitLocker, the thief would have had a nice piece of hardware and nothing else. The firm could have shrugged, ordered a replacement and moved on. The good news is there's a free tool built into Windows that dramatically reduces this risk. It's called BitLocker, and if you use Microsoft 365, you can roll it out and manage it across every company device from Microsoft Intune.
How BitLocker works?
BitLocker encrypts your entire drive. Encryption scrambles the data so it's unreadable until someone supplies the right decryption key. The clever part is where those key lives. Most modern computers have a small security chip on the motherboard called a TPM (Trusted Platform Module). BitLocker stores the key on that chip, tied to that specific machine. So, if a thief removes the hard drive and connects it to another computer, the key isn't there. All they see is scrambled data. On the original laptop, everything works as normal for you, with no extra steps at login.
What you need before you start
•	Windows edition: Pro, Enterprise or Education. Windows Home only offers a cut-down "device encryption" and can't be managed through Intune policies.
•	Windows version: Windows 11 ideally. Windows 10 reached end of support in October 2025, so treat those machines as upgrade candidates.
•	TPM chip: TPM 2.0 is the standard to aim for.
•	Management: devices enrolled in Microsoft Intune, so you can push policies and store recovery keys centrally.
Step 1: Check which devices are ready
Before you switch anything on, find out where every computer stands. Can it be encrypted? Is the TPM chip too old? Is Windows too old? Is it already encrypted? Intune has a built-in report that answers all four.
1.	Sign in to the Microsoft 365 admin center (admin.cloud.microsoft) as a global admin.
2.	Under Admin centers in the left menu, select Microsoft Intune.
<img width="883" height="503" alt="image" src="https://github.com/user-attachments/assets/61e2be6d-2088-47ff-a5ce-bacf613f7039" />
Open Microsoft Intune from the Microsoft 365 admin center
3.	In the Intune admin center, go to Devices, then Monitor.
4.	In the list of reports, open Device encryption status.
<img width="938" height="528" alt="image" src="https://github.com/user-attachments/assets/e55c7103-18ec-47a7-99f4-26f32c159a46" />
Devices > Monitor > Device encryption status
The report shows each device's encryption readiness, TPM version, Windows version and current status. Use Export to save it as your to-do list: ready devices can be encrypted now, and the rest need an upgrade first.
Step 2: Create a BitLocker policy
Now you'll build one policy that encrypts every Windows 11 device, rather than visiting each desk.
1.	In the Intune admin center, go to Endpoint security, then Disk encryption.
2.	Select Create Policy.
3.	In the Create a profile pane, set Platform to Windows and Profile to BitLocker, then select Create.
<img width="938" height="444" alt="image" src="https://github.com/user-attachments/assets/0d117921-4abb-488f-923d-0d31452ea2c9" />
Endpoint security > Disk encryption > Create a profile.
4.	On the Basics tab, give the policy a clear name, such as "Windows 11 BitLocker Encryption", and select Next.
5.	<img width="938" height="591" alt="image" src="https://github.com/user-attachments/assets/d2515e9b-15e6-45b3-ad67-de6a56a94c8d" />
Name the policy on the Basics tab
Step 3: Configure the settings
This is the heart of the policy. Work through each section on the Configuration settings tab using the values below.
BitLocker
These settings switch encryption on silently, so users aren't asked to do anything.
Setting	Value
Require Device Encryption	Enabled
Allow Warning For Other Disk Encryption	Disabled
Allow Standard User Encryption	Enabled
Configure Recovery Password Rotation	Refresh on for Entra ID-joined devices
<img width="975" height="226" alt="image" src="https://github.com/user-attachments/assets/51696507-5f18-43e4-b5ad-27121deed20c" />
<img width="942" height="564" alt="image" src="https://github.com/user-attachments/assets/e1c263f3-2e1b-4fd0-bd88-284fcbbe383c" />
BitLocker settings
BitLocker Drive Encryption
This sets how strong the encryption is. Use XTS-AES 256-bit for operating system and fixed data drives. Leave removable drives on AES-CBC 128-bit (default) so a USB stick can still be read on older Windows machines.
<img width="938" height="719" alt="image" src="https://github.com/user-attachments/assets/8388f50e-c569-4938-8924-9766db8e144a" />
Encryption method and cipher strength
Operating System Drives
This encrypts the C: drive with full encryption and unlocks it with the TPM chip alone, so users don't need a PIN at startup
<img width="975" height="261" alt="image" src="https://github.com/user-attachments/assets/abe90fb5-609f-47d5-9f1b-c0c7b3550de3" />
<img width="628" height="353" alt="image" src="https://github.com/user-attachments/assets/00795fc8-3bae-4fd5-a676-b8b79844ccb1" />
<img width="938" height="581" alt="image" src="https://github.com/user-attachments/assets/c176d86a-3ddb-44b2-b247-52f45f0bfc42" />
Operating system drive settings
Further down the same section are the recovery settings, and these matter most. They make sure the recovery key is backed up before encryption starts. Enable Choose how BitLocker-protected operating system drives can be recovered, then turn on the options to omit recovery options from the setup wizard, save recovery information, and not enable BitLocker until recovery information is stored.
<img width="938" height="789" alt="image" src="https://github.com/user-attachments/assets/e0930995-c7aa-424d-a741-6ba7e1855fd3" />
Recovery settings for the operating system drive.
On Entra ID-joined devices, the recovery key is stored in Microsoft Entra ID, even though the setting names mention AD DS.
Fixed Data Drives
Any second internal drive gets the same protection: full encryption, a 48-digit recovery password and a 256-bit recovery key, with recovery information backed up before BitLocker turns on.
<img width="938" height="619" alt="image" src="https://github.com/user-attachments/assets/c03c28b3-aa54-4413-b37c-fe5ff4c1a1d9" />
Fixed data drive settings
Removable Data Drives
Enable Control use of BitLocker on removable drives and allow users to apply BitLocker to their USB drives. Set Allow users to suspend and decrypt BitLocker protection to False, so protection can't be switched off once it's on. Then select Next.
<img width="861" height="562" alt="image" src="https://github.com/user-attachments/assets/d8a28c81-495e-4ac3-97dd-c6f26db94cb8" />
Step 4: Set scope tags
Leave the Default scope tag selected and select Next.
<img width="903" height="519" alt="image" src="https://github.com/user-attachments/assets/d40c54b7-3b70-4777-9b2a-66bbe93cb2b7" />
Scope tags
Step 5: Create a dynamic group for Windows 11 devices
You need a group to assign the policy to. A dynamic device group is ideal: new Windows 11 computers join it automatically, so they're encrypted without you lifting a finger.
1.	Open the Microsoft Entra admin center (entra.microsoft.com) and go to Groups, then All groups, then New group.
2.	Set Group type to Security and Group name to "Windows 11 Devices".
3.	Set Membership type to Dynamic Device, then select Add dynamic query
<img width="938" height="619" alt="image" src="https://github.com/user-attachments/assets/7365bf51-571e-471e-b2c4-2a08a9d112a2" />
Create a new dynamic device group in Microsoft Entra
4.	Add the two rules below, then select Save.
(device.deviceOSType -eq "Windows") and (device.deviceOSVersion -startsWith "10.0.2")
<img width="938" height="498" alt="image" src="https://github.com/user-attachments/assets/ded2cef6-d6de-4572-8472-90bbe17128d7" />
Dynamic membership rules
Windows 11 build numbers start at 22000, so its version strings begin with 10.0.2, while Windows 10 versions begin with 10.0.1.
5.	Back on the New Group page, select Create.
<img width="938" height="569" alt="image" src="https://github.com/user-attachments/assets/79e9c04a-7f08-4bf8-b71f-1e0300bed944" />
Create the group
Step 6: Assign the policy and create it
1.	Back in the Intune policy wizard, on the Assignments tab, search for the "Windows 11 Devices" group and add it.
<img width="825" height="575" alt="image" src="https://github.com/user-attachments/assets/7e9bb2d0-3136-489d-8b60-045a58776455" />
Select the Windows 11 Devices group
2.	Confirm Target type is Include, then select Next.
3.	<img width="872" height="458" alt="image" src="https://github.com/user-attachments/assets/dab02a16-9cea-43c8-ac4b-fbe81d9af689" />
Group assigned with Target type set to Include
3.	On Review + create, check the summary (37 settings, 1 scope tag and 1 group assigned), then select Create.
4.	Review + create
5.	Tip: in a live business, assign the policy to a small pilot group first. Once those devices encrypt cleanly, switch the assignment to the full group.
Step 7: Check that it worked
Give devices time to check in with Intune, then confirm the policy did its job in three places.
1. The policy is live
Go to Endpoint security, then Disk encryption. Your policy appears with Policy type BitLocker, Assigned Yes and Platform Windows.
<img width="938" height="419" alt="image" src="https://github.com/user-attachments/assets/459de762-bd57-42fb-994f-0adfaf3fe6f6" />
The BitLocker policy in Endpoint security > Disk encryption
2. The recovery key is backed up
Go to Endpoint security, then All devices, and open a device. Select BitLocker recovery keys. You'll see a key ID for the operating system drive and a Show Recovery Key link. If a user is ever locked out, this is where you'll find their key.
<img width="781" height="443" alt="image" src="https://github.com/user-attachments/assets/7082c527-06de-44c5-8a18-fcab5063ca77" />
BitLocker recovery key stored for the device
The device is encrypted
On the PC itself, open Control Panel, then System and Security, then BitLocker Drive Encryption. You should see C: BitLocker on, plus a note that some settings are managed by your system administrator.
<img width="830" height="448" alt="image" src="https://github.com/user-attachments/assets/c285b474-f091-4280-a247-9a3d95ee69aa" />
BitLocker turned on for the C: drive
Finally, rerun the Device encryption status report from Step 1 to confirm every device shows as encrypted.
The bottom line   
Laptops get lost, stolen and sold on. You can't stop that from happening, but you can decide what happens to your data when it does. With BitLocker switched on, a missing laptop is an inconvenience, not a data breach. In this guide, we used Microsoft 365 and Intune to: check which devices were ready for encryption with the Device encryption status report; build a single BitLocker policy using strong XTS-AES 256-bit encryption, unlocked silently by the TPM chip; make sure every recovery key is backed up to Microsoft Entra ID before a drive is encrypted; target a dynamic Windows 11 Devices group, so new computers are protected automatically; and confirm it worked in Intune and on the PC itself. The best part is that BitLocker is already built into Windows, and Intune is already part of many Microsoft 365 plans. There's no extra software to buy, and your users won't notice any change to how they sign in. So, here's your next step: open the Intune admin center today and run the Device encryption status report. It takes two minutes, and it will show you exactly how exposed your business is right now. And when you retire old computers, make sure they're encrypted or properly wiped before they leave the building.
