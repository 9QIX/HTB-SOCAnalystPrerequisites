# **AD Administration: Guided Lab Part II**

In this section of the guided lab, we will be completing the final tasks for the day. We have to add a computer to the domain and change the OU it resides in.

## **Connection Instructions**

For this lab, you will utilize RDP and have access to a non-domain-joined Windows host from which you can perform any actions needed to complete the lab. You will be using an RDP connection, much like in Part one.

- Click below in the Questions section to spawn the target host and obtain an IP address.

  - IP ==
  - Username == `image`
  - Password == `Academy_student_AD!`

## **Task 4 Add and Remove Computers To The Domain**

Our new users will need computers to perform their daily duties. The helpdesk has just finished provisioning them and requires us to add them to the INLANEFREIGHT domain. Since these analyst positions are new, we will need to ensure that the hosts end up in the correct OU once they join the domain so that group policy can take effect properly.

The host we need to join to the INLANEFREIGHT domain is named: `ACADEMY-IAD-W10` and has the following credentials for use to login and finish the provisioning process:

- User == `image`
- Password == `Academy_student_AD!`

Once you have access to the host, utilize your `htb-student_adm`: `Academy_student_DA!` account to join the host to the domain.

`Solution: Task 4`

## **Summary**

This wraps up our administration duties for the day. Hopefully, this lab helped reinforce the basic concepts surrounding AD management. It is always great to get hands-on experience with topics and technologies like Active Directory. This experience provides a better understanding of how it functions and how it can possibly be taken advantage of. New vulnerabilities and attacks are being released every day that affect the Windows operating system, and by extension, Active Directory. A fundamental understanding of AD, the attacks that plague it, and defensive measures will take us a long way as security Professionals.

Note: It may take 2-3 minutes for your target instance to spawn. If you receive the error message `Timeout waiting for activation` when attempting to connect via RDP, wait a few seconds and run the command again. Furthermore, loading the Active Directory PowerShell module or the MMC snap-ins may take a bit longer on the first run.
