# Docker Networking

**Demo Overview**

This demo demonstrates Docker networking concepts by creating three containers — Entry, Exit, and Account — setting up a custom bridge network for the Account container, and verifying connectivity between containers using ping tests.

---

**Concepts Covered**

  - Default Bridge Network

  - Custom Bridge Network

  - Container-to-Container Communication

  - Network Inspection & Ping Testing

---

**Steps I followed in this demo**

---

**Step 1 : Creating Entry Container and Bank Conatiner**

   Command - docker run -d --name entry alpine and  docker run -d --name bank alpine 

---

<img width="1264" height="202" alt="image" src="https://github.com/user-attachments/assets/f971d68b-5b37-4176-a3e6-2f2c728f5de9" />


---

**Step 2 : Checking the IP Address of the Containers** 

   - Command - docker inspect entry & docker imspect bank

   - Ip address of this two containers will be in same subnet because there are having an default Bridge network 

   
**Entry Conatiner**

---

<img width="916" height="472" alt="image" src="https://github.com/user-attachments/assets/f5f4f461-dfc5-4326-be9f-05406be5d63c" />

---

**Exit Container**

---

<img width="900" height="586" alt="image" src="https://github.com/user-attachments/assets/a285e6b5-166b-493d-ac12-7fbc480d73f2" />

---

**Step 3 : Pinning the IP Address of entry container in exit container by logging into exit container**

**Login to exit** 

---

<img width="1221" height="95" alt="image" src="https://github.com/user-attachments/assets/16f4fc4d-8908-4bf8-adbb-4dd9eba000b5" />

---

**Adding ping to the container**

---

<img width="784" height="328" alt="image" src="https://github.com/user-attachments/assets/f17cdf42-fa27-4296-a688-5f191afb8792" />

---

**Pining the IP Address of entry Conatiner in the exit container**

---

<img width="979" height="297" alt="image" src="https://github.com/user-attachments/assets/ef06ef0c-26d7-4044-a7e8-cd44abb68a2e" />

---

**Step 3 : Creating an Account Container and Adding custom bridge network**

**Account Container**

---

<img width="1166" height="225" alt="image" src="https://github.com/user-attachments/assets/0ee2be41-8f45-45df-af88-84fe2998cb1f" />

---

**Create a Custom Bridge Network**

---

<img width="1202" height="57" alt="image" src="https://github.com/user-attachments/assets/e94e30ba-a4c5-4898-8c9a-3242a4b711b7" />


---

**Connect Account Container to Custom Bridge Network**

---

<img width="1232" height="196" alt="image" src="https://github.com/user-attachments/assets/f167b545-b9c6-4406-8572-46d79b5dffa7" />

---


**Step 4 : Getting the IP Address of the account Container to pining in the exit container**

---

<img width="876" height="544" alt="image" src="https://github.com/user-attachments/assets/45c82587-ab11-4c93-83c8-a6d25e736a00" />

--- 

**Step 5 : Logging to exit container and pining the IP Address of Account Container**

---

<img width="1136" height="218" alt="image" src="https://github.com/user-attachments/assets/d1fd95f9-f7b0-443e-90e9-9f482f19e9e1" />

---

# Docker Bridge Networks — Summary

## Default Bridge Network

- Automatically created when Docker is installed
- All containers connect to it **by default** if no network is specified
- Containers can communicate using **IP addresses only** (not by name)
- Less secure — all containers on the host can talk to each other
- Cannot be customized easily

---

## Custom Bridge Network

- Manually created by the user
- Containers can communicate using **both IP address and container name**
- More secure — only containers explicitly connected can communicate
- Supports better **isolation and control**
- Can be customized (subnet, gateway, etc.)

