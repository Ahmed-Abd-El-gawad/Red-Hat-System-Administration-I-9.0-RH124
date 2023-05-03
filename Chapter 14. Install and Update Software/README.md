# Chapter 14. Install and Update Software

| content |
| --- |
| [14.1 Register Systems for Red Hat Support](#14.1) |
| [14.3 Explain and Investigate RPM Software Packages](#14.3) |
| [14.5 Install and Update Software Packages with DNF](#14.5) |
| [14.7 Enable DNF Software Repositories](#14.7) |


<a name="14.1"></a>
## 14.1 Register Systems for Red Hat Support

| Content |
| --- |
| [dnf](#dnf) |
| [Simple Content Access](#SCA) |
| [subscription-manager](#subscription-manager) |

<a name="dnf"></a>
* ```dnf``` command obtain software packages and updates through a content distribution network that the Red Hat Content Delivery Network provides.

<a name="SCA"></a>
* Simple Content Access (SCA) is a Red Hat subscription management capability. When you enable SCA for your organization, the entitlement process is simplified. SCA eliminates the requirement to attach subscriptions at a per-system level. You register your systems, enable the repositories that each system needs, and begin installing software packages.

<a name="subscription-manager"></a>
* Use the ```subscription-manager``` command to register a system without using a graphical environment. The ```subscription-manager``` command automatically attaches a system to the best-matched compatible subscriptions for the system.

  * Register a system by using the credentials of the Red Hat Customer Portal as the ```root``` user:
    ```console
    [root@host ~]# subscription-manager register --username <yourusername>
    Registering to: subscription.rhsm.redhat.com:443/subscription
    Password: yourpassword
    The system has been registered with ID: 1457f7e9-f37e-4e93-960a-c94fe08e1b4f
    The registered system name is: host.example.com
    ```
  * View available subscriptions for your Red Hat account:
    ```console
    [root@host ~]# subscription-manager list --available
    -------------------------------------------
        Available Subscriptions
    -------------------------------------------
    ...output omitted...
    ```
  * Auto-attach a subscription:
    ```console
    [root@host ~]# subscription-manager attach --auto
    ...output omitted...
    ```
  * Alternatively, attach a subscription from a specific pool from the list of available subscriptions:
    ```console
    [root@host ~]# subscription-manager attach --pool=poolID
    ...output omitted...
    ```
  * View consumed subscriptions:
    ```console
    [root@host ~]# subscription-manager list --consumed
    ...output omitted...
    ```
  * Unregister a system:
    ```console
    [root@host ~]# subscription-manager unregister
    Unregistering from: subscription.rhsm.redhat.com:443/subscription
    System has been unregistered.
    ```


<a name="14.3"></a>
## 14.3 Explain and Investigate RPM Software Packages

| Content |
| --- |

<a name=""></a>
* 


<a name="14.5"></a>
## 14.5 Install and Update Software Packages with DNF

| Content |
| --- |

<a name=""></a>
* 


<a name="14.7"></a>
## 14.7 Enable DNF Software Repositories

| Content |
| --- |

<a name=""></a>
* 


