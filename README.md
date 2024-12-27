# Software-Cheat-Sheets


| Document  | Github  | Google Viewer |
| :------------ |:---------------:| -----:|
| Design Patterns       | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/Design%20Patterns%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/Design%20Patterns%20Cheat%20Sheet.pdf) |
| Git      | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/Git%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/Git%20Cheat%20Sheet.pdf) |
| Object Oriented Design | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/Object%20Oriented%20Design%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/Object%20Oriented%20Design%20Cheat%20Sheet.pdf) |
| QT Creator | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/Qt%20Creator%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/Qt%20Creator%20Cheat%20Sheet.pdf) |
| SOLID Principles | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/SOLID%20Principle%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/SOLID%20Principle%20Cheat%20Sheet.pdf) |
| UML | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/UML%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/UML%20Cheat%20Sheet.pdf) |
| UML basic | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/UML%20-basic-%20Cheat%20Sheet.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/UML%20-basic-%20Cheat%20Sheet.pdf) |
| C++ for Absolute Beginners | [Link](https://github.com/Furkan-Dursun/Software-Cheat-Sheets/blob/main/C%2B%2B%20for%20Absolute%20Beginners.pdf) | [Link](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/Furkan-Dursun/Software-Cheat-Sheets/main/C%2B%2B%20for%20Absolute%20Beginners.pdf) |




Here’s a well-structured `README.md` code snippet for the process:

```markdown
# MongoDB Backup and Restore with Docker

This guide explains how to create a backup from one MongoDB container and restore it to another using Docker.

---

## Steps

### 1. Create a Backup from the First MongoDB Container

Run the following command to create a compressed backup (`backup.archive`) of the MongoDB database inside the first container:

```bash
docker exec -it geothermal_mongodb_1 bash -c "mongodump --archive=/data/db/backup.archive --gzip --uri='mongodb://developer:secret@localhost:27017/storage-db?authSource=admin'"
```

### 2. Copy the Backup File to the Host Machine

Use `docker cp` to transfer the backup file from the container to the host machine:

```bash
docker cp geothermal_mongodb_1:/data/db/backup.archive .
```

### 3. Start a New MongoDB Container

Create and run a new MongoDB container:

```bash
docker run -d --name geothermal_mongodb_2 -p 27018:27017 mongo:5
```

### 4. Copy the Backup File to the New Container

Transfer the backup file from the host machine to the new container:

```bash
docker cp backup.archive geothermal_mongodb_2:/backup.archive
```

### 5. Restore the Backup in the New Container

Run the `mongorestore` command inside the new container to restore the backup:

```bash
docker exec -it geothermal_mongodb_2 mongorestore --archive=/backup.archive --gzip
```

---

## Verification

- **Verify Backup Creation:** Check the presence of `backup.archive` in the first container.
- **Verify Restoration:** Confirm the database is restored in the new container by connecting to it and checking the collections.

```bash
docker exec -it geothermal_mongodb_2 mongo
```

Use the `show dbs` and `use <database>` commands to explore the restored database.

---

## Notes

- Ensure the MongoDB versions in both containers are compatible.
- Replace `developer` and `secret` with the appropriate username and password for your database.
- Use the appropriate port numbers and container names based on your setup.

```
