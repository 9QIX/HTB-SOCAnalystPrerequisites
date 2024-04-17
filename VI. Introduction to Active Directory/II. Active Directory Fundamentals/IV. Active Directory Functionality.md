# Active Directory Functionality

As mentioned before, there are five Flexible Single Master Operation (FSMO) roles. These roles can be defined as follows:

| Roles                    | Description                                                                                                                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Schema Master            | This role manages the read/write copy of the AD schema, which defines all attributes that can apply to an object in AD.                                                                                                                                                                                            |
| Domain Naming Master     | Manages domain names and ensures that two domains of the same name are not created in the same forest.                                                                                                                                                                                                             |
| Relative ID (RID) Master | The RID Master assigns blocks of RIDs to other DCs within the domain that can be used for new objects. The RID Master helps ensure that multiple objects are not assigned the same SID. Domain object SIDs are the domain SID combined with the RID number assigned to the object to make the unique SID.          |
| PDC Emulator             | The host with this role would be the authoritative DC in the domain and respond to authentication requests, password changes, and manage Group Policy Objects (GPOs). The PDC Emulator also maintains time within the domain.                                                                                      |
| Infrastructure Master    | This role translates GUIDs, SIDs, and DNs between domains. This role is used in organizations with multiple domains in a single forest. The Infrastructure Master helps them to communicate. If this role is not functioning properly, Access Control Lists (ACLs) will show SIDs instead of fully resolved names. |

Depending on the organization, these roles may be assigned to specific DCs or as defaults each time a new DC is added. Issues with FSMO roles will lead to authentication and authorization difficulties within a domain.

## Domain and Forest Functional Levels

Microsoft introduced functional levels to determine the various features and capabilities available in Active Directory Domain Services (AD DS) at the domain and forest level. They are also used to specify which Windows Server operating systems can run a Domain Controller in a domain or forest. This and this article describe both the domain and forest functional levels from Windows 2000 native to Windows Server 2012 R2. Below is a quick overview of the differences in domain functional levels from Windows 2000 native up to Windows Server 2016, aside from all default Active Directory Directory Services features from the level just below it (or just the default AD DS features in the case of Windows 2000 native.)

| Domain Functional Level | Features Available                                                                                                                                                       | Supported Domain Controller Operating Systems                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| Windows 2000 native     | Universal groups for distribution and security groups, group nesting, group conversion (between security and distribution and security groups), SID history.             | Windows Server 2008 R2, Windows Server 2008, Windows Server 2003, Windows 2000                                |
| Windows Server 2003     | Netdom.exe domain management tool, lastLogonTimestamp attribute introduced, well-known users and computers containers, constrained delegation, selective authentication. | Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 |
| Windows Server 2008     | Distributed File System (DFS) replication support, Advanced Encryption Standard (AES 128 and AES 256) support for the Kerberos protocol, Fine-grained password policies  | Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008                      |
| Windows Server 2008 R2  | Authentication mechanism assurance, Managed Service Accounts                                                                                                             | Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2                                           |
| Windows Server 2012     | KDC support for claims, compound authentication, and Kerberos armoring                                                                                                   | Windows Server 2012 R2, Windows Server 2012                                                                   |
| Windows Server 2012 R2  | Extra protections for members of the Protected Users group, Authentication Policies, Authentication Policy Silos                                                         | Windows Server 2012 R2                                                                                        |
| Windows Server 2016     | Smart card required for interactive logon new Kerberos features and new credential protection features                                                                   | Windows Server 2019 and Windows Server 2016                                                                   |

A new functional level was not added with the release of Windows Server 2019. However, Windows Server 2008 functional level is the minimum requirement for adding Server 2019 Domain Controllers to an environment. Also, the target domain has to use DFS-R for SYSVOL replication.

Forest functional levels have introduced a few key capabilities over the years:

| Version                | Capabilities                                                                                                           |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Windows Server 2003    | saw the introduction of the forest trust, domain renaming, read-only domain controllers (RODC), and more.              |
| Windows Server 2008    | All new domains added to the forest default to the Server 2008 domain functional level. No additional new features.    |
| Windows Server 2008 R2 | Active Directory Recycle Bin provides the ability to restore deleted objects when AD DS is running.                    |
| Windows Server 2012    | All new domains added to the forest default to the Server 2012 domain functional level. No additional new features.    |
| Windows Server 2012 R2 | All new domains added to the forest default to the Server 2012 R2 domain functional level. No additional new features. |
| Windows Server 2016    | Privileged access management (PAM) using Microsoft Identity Manager (MIM).                                             |

## Trusts

A trust is used to establish forest-forest or domain-domain authentication, allowing users to access resources in (or administer) another domain outside of the domain their account resides in. A trust creates a link between the authentication systems of two domains.

There are several trust types.

| Trust Type   | Description                                                                                                                                                            |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Parent-child | Domains within the same forest. The child domain has a two-way transitive trust with the parent domain.                                                                |
| Cross-link   | a trust between child domains to speed up authentication.                                                                                                              |
| External     | A non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust. This type of trust utilizes SID filtering.     |
| Tree-root    | a two-way transitive trust between a forest root domain and a new tree root domain. They are created by design when you set up a new tree root domain within a forest. |
| Forest       | a transitive trust between two forest root domains.                                                                                                                    |

![alt text](/Images/image-74.png)

Trusts can be transitive or non-transitive.

A transitive trust means that trust is extended to objects that the child domain trusts.

In a non-transitive trust, only the child domain itself is trusted.

Trusts can be set up to be one-way or two-way (bidirectional).

- In bidirectional trusts, users from both trusting domains can access resources.
- In a one-way trust, only users in a trusted domain can access resources in a trusting domain, not vice-versa. The direction of trust is opposite to the direction of access.

Often, domain trusts are set up improperly and provide unintended attack paths. Also, trusts set up for ease of use may not be reviewed later for potential security implications. Mergers and acquisitions can result in bidirectional trusts with acquired companies, unknowingly introducing risk into the acquiring company's environment. It is not uncommon to be able to perform an attack such as Kerberoasting against a domain outside the principal domain and obtain a user that has administrative access within the principal domain.
