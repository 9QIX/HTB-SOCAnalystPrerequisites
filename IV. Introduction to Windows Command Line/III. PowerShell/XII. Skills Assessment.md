# Skills Assessment

During a penetration test with our team, we were instructed to collect some information for a non-critical Windows host. Our team has recently gained access to this host, which contains many users. For the team to focus on more complex tasks, we have been asked to take a closer look at this host. The main focus is on finding user names and passwords. This should enable us to examine the system in such a way that we can find out and document all user passwords.

Each question has a corresponding user with whom you will need to authenticate to complete the questions. In each challenge, you may be asked to perform specific actions, use specific executables, or find information on the host to get the flag for that question.

In most instances, the flag for the previous user must be used as the SSH password for the following user (i.e., the flag for user2 is the password for user3 to SSH in, and so on).

If you wish, play around and see if you can find multiple ways to achieve the same output as you got earlier from each question. Do not forget to document your one-liners, scripts, or general notes on how you went about finding specific information.

## Questions

1. The flag will print in the banner upon successful login on the host via SSH.

2. Access the host as user1 and read the contents of the file "flag.txt" located in the users Desktop.

3. If you search and find the name of this host, you will find the flag for user2.

4. How many hidden files exist on user3's Desktop?

5. User4 has a lot of files and folders in their Documents folder. The flag can be found within one of them.

6. How many users exist on this host? (Excluding the DefaultAccount and WDAGUtility)

7. For this level, you need to find the Registered Owner of the host. The Owner name is the flag.
