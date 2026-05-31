# Docker Volume Handsons

---

**Volume Creation**

  - Command - docker volume create (name of the Volume)

---

<img width="915" height="57" alt="image" src="https://github.com/user-attachments/assets/8f9f421b-0ace-4109-9efd-43a79713682b" />


---

**Volume inspect which means details of volume**

   - Command - docker volume inspect (name of the Volume)

---

<img width="1004" height="302" alt="image" src="https://github.com/user-attachments/assets/c797f487-00e9-4323-a2b5-9fa70bc60f33" />


---

**Volume Deletion**

  - Command - docker volume rm (name of the Volume)

---

<img width="1023" height="110" alt="image" src="https://github.com/user-attachments/assets/6b1d296a-7393-4e1b-a3ab-eeb4d7bfb174" />


---

**Mounting Docker volume to the specifuc container in the detached mode**

  - Command - docker run -d --mount source=(name of the volume created), target=/app (image name)

---

<img width="1333" height="57" alt="image" src="https://github.com/user-attachments/assets/57930ea9-bad4-4d50-8632-b803df2751c0" />


---

**Verification**

---

<img width="879" height="267" alt="image" src="https://github.com/user-attachments/assets/f65ad069-9858-4f28-bf86-907a885cba3e" />

---


**Inspecting the Docker**

  - Command - docker image (container ID)

---

<img width="1258" height="602" alt="image" src="https://github.com/user-attachments/assets/8deedbf8-09b2-44ae-b4a2-6bf02494f76d" />


---

**To Stop and Start Conatiner**

  - To stop or start the container we need to first need to stop the container, After that we need to stop or start the Volumes

**Without deleting the Conatiner**

---

<img width="1880" height="81" alt="image" src="https://github.com/user-attachments/assets/bd01a4e8-a890-49cb-a21a-173f28f986a7" />

---


**After Deleting the Container**

---

<img width="1128" height="497" alt="image" src="https://github.com/user-attachments/assets/4930d205-b0ad-45d1-88b3-090fabf3ed4d" />

---
