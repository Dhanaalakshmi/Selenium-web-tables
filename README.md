# Selenium web tables
# Name: DHANA LAKSHMI A
# Reg no : 212223040033
# Code 
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait, Select

driver = webdriver.Chrome()
driver.maximize_window()
wait = WebDriverWait(driver, 15)
driver.get("https://assertqa.com/practice/webtables")

# Show all rows on one page (default is 5)
Select(wait.until(lambda d: d.find_element(
    By.XPATH, "//select[option[normalize-space()='25']]"))).select_by_visible_text("25")
wait.until(lambda d: len(d.find_elements(By.XPATH, "//table/tbody/tr")) == 15)

headers = [h.get_attribute("textContent").strip() for h in driver.find_elements(By.XPATH, "//table/thead/tr/th")]
rows = driver.find_elements(By.XPATH, "//table/tbody/tr")
data = [[c.get_attribute("textContent").strip() for c in r.find_elements(By.TAG_NAME, "td")] for r in rows]
# Each data row = [#, First, Last, Email, Age, Salary, Dept, Status]

# TC01 - column headings
print("TC01 Headings:", headers)
assert len(headers) >= 7
print("TC01 PASS")

# TC02 - first data row
print("TC02 First row:", data[0])
assert data[0][1] == "John"
print("TC02 PASS")

# TC03 - last data row
print("TC03 Last row:", data[-1])
assert len(data[-1]) >= 8
print("TC03 PASS")

# TC04 - search by last name
name = "Williams"
found = [r for r in data if r[2] == name]
print("TC04 Match:", found)
assert found
print("TC04 PASS")

# TC05 - all emails
emails = [r[3] for r in data]
print("TC05 Emails:", *emails, sep="\n  ")
assert all("@" in e for e in emails)
print("TC05 PASS")

# TC06 - highest amount (page has Salary, no Due column)
top = max(data, key=lambda r: int(r[5].replace("$", "").replace(",", "")))
print("TC06 Highest salary:", top[1], top[2], top[5])
assert top[2] == "Jones"      # David Jones, $120,000
print("TC06 PASS")

# TC07 - link exists
links = driver.find_elements(By.XPATH, "//a[normalize-space()='Blog']")
print("TC07 PASS" if links else "TC07 FAIL")
assert links

# TC08 - count data rows (header excluded)
print("TC08 Row count:", len(data))
assert len(data) == 15
print("TC08 PASS")

driver.quit()
```

# Output
<img width="1361" height="925" alt="image" src="https://github.com/user-attachments/assets/82034479-18bb-4175-9d23-bec717d6e700" />

