# 🕵️‍♂️ SQL Security Investigation: Tracking Suspicious Logins

## The Scenario
My journey into cybersecurity is just beginning, and I am quickly learning that defending systems requires knowing how to investigate the data they leave behind. To put my growing skills to the test, I took on a simulated challenge: investigate a potential security incident involving suspicious login attempts and track down vulnerable employee machines that needed urgent security patches. 

Armed with SQL, I dove into the organization's `employees` and `log_in_attempts` tables to see what the data could tell me. Here is how the investigation unfolded.

## 1. Tracking After-Hours Activity
The first clue in the investigation was a report of strange activity happening after business hours. I needed to see exactly who was failing to get into the system after everybody else had gone home. I wrote a query to filter for failed logins that occurred strictly after 6:00 PM.

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00:00' AND success = 0;
```
<img width="1470" height="836" alt="1st " src="https://github.com/user-attachments/assets/1866224c-3aad-4da3-8854-de7ff6c213c2" />


By using the AND operator, I was able to tell the database to only show me records where the login_time was late in the evening and the success value was 0 (indicating a failed attempt).

## 2. Zeroing In on Specific Dates
Next, I got a tip about a specific suspicious event that happened on May 9th, 2022. To get the full context, I wanted to pull the logs for that exact day, as well as the day prior, to see if there was any build-up to the event.

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```
<img width="1340" height="836" alt="2nd" src="https://github.com/user-attachments/assets/191675d6-ba84-40ec-ac7e-3534025056fd" />


Using the OR operator allowed me to cast a slightly wider net, pulling in all the login data that matched either of those two target dates.

## 3. Tracing Out-of-Country Logins
The plot thickened when it was determined that the suspicious activity definitely wasn't originating from the organization's usual network in Mexico. I needed to isolate all the international login attempts to find the true source.

```sql
SELECT *
FROM log_in_attempts
WHERE country NOT LIKE 'MEX%';
```
<img width="1338" height="835" alt="3rd" src="https://github.com/user-attachments/assets/013a1c1a-a311-480b-a0f1-a6696c55342d" />


Because the country might be entered as "MEX" or "MEXICO", I used the LIKE keyword and the % wildcard to cover all variations. Wrapping that logic in a NOT operator perfectly filtered out the normal traffic, leaving only the external login attempts for me to analyze.


## 4. Securing the Marketing Department
Switching gears from investigating to securing, I was tasked with locating specific machines that were vulnerable and needed a critical update. The first target was the Marketing team located in the East building.

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```
<img width="846" height="297" alt="4th" src="https://github.com/user-attachments/assets/5f10d240-0555-46a6-97b4-f163fbc07221" />


Here, the AND operator ensured I only pulled records for employees who met both conditions: working in Marketing, and sitting in an office that started with the word "East".


## 5. Expanding the Security Patch
The patching process continued. Next up on the list were the Sales and Finance departments, who needed a different type of security update deployed to their machines.

```sql
SELECT *
FROM employees
WHERE department = 'Sales' OR department = 'Finance';
```
<img width="1266" height="837" alt="5th" src="https://github.com/user-attachments/assets/63d6f6e8-ac56-4cfb-86c3-cf9cf052cecb" />


The OR operator made quick work of this, retrieving the employee data if they belonged to either of the vulnerable departments.

## 6. The Final Rollout
Finally, we had to roll out a broad update to everyone in the company except the Information Technology department, since the IT team had already secured their own systems.

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```
<img width="1333" height="837" alt="6th" src="https://github.com/user-attachments/assets/bf07345e-32de-40a8-9188-6b8b76d1d89d" />


Using the NOT operator on the department column allowed me to easily filter out the IT staff and return a clean list of every other employee who still needed the patch.


## Summary
This simulated investigation was a fantastic, hands-on way to see how SQL is used in the real world of cybersecurity. By combining logical operators like AND, OR, and NOT with LIKE wildcards, I was able to sift through logs, pinpoint suspicious geographic locations, and target vulnerable machines for updates. Using database queries to "solve the case" made these foundational data analysis concepts really click for me!


