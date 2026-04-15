# Platform Engineering Interview Preparation: Complete Program

## 3-Week Intensive: Programming Fundamentals → Platform Architecture

**Your Diagnosis:** Strong operational knowledge (Terraform, Kubernetes, CI/CD) but weak computational thinking (loops, functions, data structures, control flow).

**Root Cause of Round 2 Failure:** Generated code with Claude that you couldn’t explain because you lack programming fluency.

**Solution:** Build programming fundamentals first, then apply to platform patterns.

**Time Commitment:** 2 hours/day for 3 weeks  
**Critical Rule:** NO Claude/ChatGPT for code generation during exercises. You must write code yourself.

-----

# Week 1: Programming Fundamentals (Python)

**Goal:** Understand variables, control flow, functions, data structures, file I/O, error handling.

**Why Python:** Simple syntax, widely used in platform automation (Lambda, ops scripts, Terraform wrappers).

**Format:** 30 min learning + 60 min coding + 30 min review

-----

## Day 1: Variables, Types, Basic I/O

### **Core Concepts**

**Variables and Assignment:**

```python
# Variables store data
name = "Claude"           # string
age = 5                   # integer
price = 19.99            # float
is_active = True         # boolean

# Variables can change
count = 10
count = count + 1  # Now 11
count += 1         # Shorthand, now 12

# String operations
greeting = "Hello " + name        # Concatenation
message = f"Name: {name}, Age: {age}"  # f-string formatting
```

**Input and Output:**

```python
# Print to console
print("Hello world")
print(f"The answer is {42}")

# Read user input (always returns string)
user_name = input("Enter your name: ")
age_str = input("Enter your age: ")
age_int = int(age_str)  # Convert string to integer
```

**Type Conversion:**

```python
# String to number
num = int("42")        # String to integer
price = float("19.99")  # String to float

# Number to string
text = str(42)         # Integer to string

# Check type
print(type(age))  # <class 'int'>
```

### **Learning Exercise (30 min)**

Type this code and run it:

```python
# AWS cost calculator
print("=== AWS Monthly Cost Calculator ===")

instance_count = input("Number of EC2 instances: ")
instance_count = int(instance_count)

price_per_instance = input("Cost per instance ($/month): ")
price_per_instance = float(price_per_instance)

total_cost = instance_count * price_per_instance

print(f"\nTotal monthly cost: ${total_cost:.2f}")
```

**Run it. Experiment. Change values. Break it. Fix it.**

### **Practice Exercises (60 min)**

**Exercise 1.1: Personal Info Collector**

```python
# Ask user for: name, age, favorite programming language
# Print: "Hello [name], you are [age] years old and you like [language]"
```

**Exercise 1.2: Temperature Converter**

```python
# Ask user for temperature in Celsius
# Convert to Fahrenheit: F = (C * 9/5) + 32
# Print: "[C]°C is [F]°F"
```

**Exercise 1.3: RDS Cost Estimator**

```python
# Ask user for:
# - Number of RDS instances
# - Instance class (e.g., db.t3.medium)
# - Storage size in GB
# Calculate cost:
# - db.t3.medium = $50/month
# - Storage = $0.10/GB/month
# Print total monthly cost
```

**Exercise 1.4: Kubernetes Pod Capacity**

```python
# Ask user for:
# - Number of worker nodes
# - Pods per node
# Calculate and print: Total pod capacity across cluster
```

### **Review Checklist (30 min)**

- [ ] I can create variables and assign values
- [ ] I can convert between string, int, and float
- [ ] I can use f-strings for formatted output
- [ ] I can read user input and use it in calculations
- [ ] I understand the difference between “42” (string) and 42 (integer)

**If you can’t check all boxes, redo Day 1 tomorrow.**

-----

## Day 2: Conditionals (if/elif/else)

### **Core Concepts**

**Basic if statements:**

```python
age = 18

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

**Multiple conditions with elif:**

```python
status_code = 404

if status_code == 200:
    print("Success")
elif status_code == 404:
    print("Not Found")
elif status_code == 500:
    print("Server Error")
else:
    print("Other status")
```

**Comparison Operators:**

```python
==  # Equal to
!=  # Not equal to
>   # Greater than
<   # Less than
>=  # Greater than or equal
<=  # Less than or equal
```

**Logical Operators:**

```python
# AND - both must be true
if age >= 18 and has_license:
    print("Can drive")

# OR - at least one must be true
if status_code == 500 or status_code == 503:
    print("Server error")

# NOT - inverts boolean
if not is_admin:
    print("Access denied")
```

**Nested conditions:**

```python
if environment == "prod":
    if approved:
        print("Deploying to production")
    else:
        print("Approval required")
else:
    print("Deploying to dev/stage")
```

### **Learning Exercise (30 min)**

```python
# HTTP status classifier
print("=== HTTP Status Checker ===")

status = int(input("Enter HTTP status code: "))

if status >= 200 and status < 300:
    print("✅ Success")
elif status >= 300 and status < 400:
    print("↪️  Redirect")
elif status >= 400 and status < 500:
    print("❌ Client Error")
elif status >= 500 and status < 600:
    print("💥 Server Error")
else:
    print("⚠️  Unknown status code")
```

### **Practice Exercises (60 min)**

**Exercise 2.1: Pod Status Checker**

```python
# Ask user for pod status: "Running", "Pending", "Failed", "Unknown"
# Print:
# - Running: "✅ Pod is healthy"
# - Pending: "⏳ Pod is starting"
# - Failed: "❌ Pod has crashed"
# - Unknown: "⚠️  Check pod status manually"
```

**Exercise 2.2: EC2 Instance Type Selector**

```python
# Ask user for:
# - Number of CPU cores needed
# - Amount of RAM (GB) needed
# Recommend instance type:
# - 2 cores, 4GB → "t3.medium"
# - 2 cores, 8GB → "t3.large"
# - 4 cores, 8GB → "c5.xlarge"
# - 4 cores, 16GB → "m5.xlarge"
# - Otherwise → "Custom configuration needed"
```

**Exercise 2.3: Terraform Plan Analyzer**

```python
# Ask user for number of:
# - Resources to create
# - Resources to destroy
# - Resources to modify
# Calculate risk score:
# - Destroy = 10 points each
# - Modify = 2 points each
# - Create = 1 point each
# Print risk level:
# - Score 0-5: "Low risk"
# - Score 6-20: "Medium risk - review recommended"
# - Score 21+: "High risk - approval required"
```

**Exercise 2.4: Database Password Validator**

```python
# Ask user for password
# Check if password meets requirements:
# - Length >= 12 characters
# - Contains at least one number
# - Contains at least one uppercase letter
# Print: "Valid password" or list what's missing
#
# Hints:
# len(password) gives length
# password.isdigit() checks if all digits
# any(c.isdigit() for c in password) checks if any character is digit
# any(c.isupper() for c in password) checks if any character is uppercase
```

### **Review Checklist (30 min)**

- [ ] I can write if/elif/else statements
- [ ] I understand comparison operators (==, !=, >, <, etc.)
- [ ] I can use logical operators (and, or, not)
- [ ] I can nest conditions when needed
- [ ] I can translate requirements into conditional logic

-----

## Day 3: Loops (for, while)

### **Core Concepts**

**For loops with range:**

```python
# Print numbers 0-4
for i in range(5):
    print(i)

# Start, stop, step
for i in range(1, 10, 2):  # 1, 3, 5, 7, 9
    print(i)
```

**For loops with lists:**

```python
services = ["api", "auth", "payments"]

for service in services:
    print(f"Checking {service}...")
```

**While loops:**

```python
# Continue until condition is false
count = 0
while count < 5:
    print(count)
    count += 1  # Don't forget to increment!
```

**Loop control:**

```python
# Break - exit loop immediately
for i in range(10):
    if i == 5:
        break  # Stop at 5
    print(i)

# Continue - skip to next iteration
for i in range(5):
    if i == 2:
        continue  # Skip 2
    print(i)  # Prints: 0, 1, 3, 4
```

**Accumulation pattern:**

```python
# Sum numbers
total = 0
for i in range(1, 11):
    total += i  # Add each number
print(f"Sum: {total}")  # 55

# Count matching items
error_count = 0
for status in [200, 500, 200, 404, 200]:
    if status >= 400:
        error_count += 1
print(f"Errors: {error_count}")  # 2
```

### **Learning Exercise (30 min)**

```python
# Pod health checker simulator
pods = ["api-pod-1", "auth-pod-2", "db-pod-3", "cache-pod-4"]

print("=== Kubernetes Pod Health Check ===")

healthy_count = 0
failed_count = 0

for pod in pods:
    # Simulate health check (in reality, would call Kubernetes API)
    status = input(f"Status of {pod} (running/failed): ")
    
    if status == "running":
        print(f"  ✅ {pod} is healthy")
        healthy_count += 1
    else:
        print(f"  ❌ {pod} has failed")
        failed_count += 1

print(f"\nSummary: {healthy_count} healthy, {failed_count} failed")

if failed_count > 0:
    print("⚠️  Action required: Check failed pods")
```

### **Practice Exercises (60 min)**

**Exercise 3.1: Service Health Monitor**

```python
# Create a list of service URLs:
services = [
    "https://api.example.com/health",
    "https://auth.example.com/health",
    "https://payments.example.com/health",
    "https://database.example.com/health"
]

# For each service:
# - Print "Checking [service]..."
# - Ask user for status code
# - If 200: print "✅ UP"
# - Otherwise: print "❌ DOWN"
# Count total UP and DOWN services
# Print summary
```

**Exercise 3.2: Error Rate Calculator**

```python
# Given list of HTTP status codes:
status_codes = [200, 200, 500, 200, 404, 200, 500, 200, 200, 503]

# Count:
# - Total requests
# - Success (200-299)
# - Client errors (400-499)
# - Server errors (500-599)
# Calculate error rate: (client_errors + server_errors) / total * 100
# Print summary with percentages
```

**Exercise 3.3: Deployment Countdown**

```python
# Ask user for countdown time in seconds (e.g., 10)
# Count down from that number to 0
# Print each number with 1 second delay between
# At 0, print "🚀 Deployment started!"
#
# Hint: import time, then use time.sleep(1) to wait 1 second
```

**Exercise 3.4: Terraform Resource Counter**

```python
# Given list of Terraform resources:
resources = [
    "aws_instance.web",
    "aws_instance.api",
    "aws_db_instance.main",
    "aws_s3_bucket.assets",
    "aws_s3_bucket.logs",
    "aws_lb.main"
]

# Count how many of each resource type:
# - aws_instance: ?
# - aws_db_instance: ?
# - aws_s3_bucket: ?
# - aws_lb: ?
#
# Hint: Use .startswith("aws_instance") to check prefix
```

### **Review Checklist (30 min)**

- [ ] I can write for loops with range()
- [ ] I can iterate over a list with for loops
- [ ] I can use while loops with proper increment
- [ ] I understand break and continue
- [ ] I can accumulate values (count, sum) in a loop

-----

## Day 4: Lists and Dictionaries

### **Core Concepts**

**Lists - Ordered Collections:**

```python
# Create list
services = ["api", "auth", "payments"]

# Access by index (0-based)
print(services[0])   # "api"
print(services[-1])  # "payments" (last item)

# Add items
services.append("database")     # Add to end
services.insert(1, "cache")     # Insert at position 1

# Remove items
services.remove("auth")         # Remove by value
last = services.pop()           # Remove and return last item

# List operations
length = len(services)          # Number of items
services.sort()                 # Sort in place
reversed = services[::-1]       # Reverse (creates new list)

# Check membership
if "api" in services:
    print("API service exists")
```

**Dictionaries - Key-Value Pairs:**

```python
# Create dictionary
service_status = {
    "api": "UP",
    "auth": "DOWN",
    "payments": "UP"
}

# Access by key
print(service_status["api"])  # "UP"

# Add or update
service_status["database"] = "UP"   # Add new key
service_status["auth"] = "UP"       # Update existing

# Remove
del service_status["payments"]

# Check if key exists
if "api" in service_status:
    print("API key exists")

# Get with default
status = service_status.get("unknown", "NOT_FOUND")

# Iterate
for service, status in service_status.items():
    print(f"{service}: {status}")

# Keys and values
all_services = service_status.keys()    # List of keys
all_statuses = service_status.values()  # List of values
```

**List of Dictionaries:**

```python
# Common pattern in platform engineering
pods = [
    {"name": "api-pod-1", "status": "Running", "restarts": 0},
    {"name": "auth-pod-2", "status": "Failed", "restarts": 3},
    {"name": "db-pod-3", "status": "Running", "restarts": 0}
]

# Access
print(pods[0]["name"])  # "api-pod-1"

# Filter
failed_pods = []
for pod in pods:
    if pod["status"] == "Failed":
        failed_pods.append(pod)
```

### **Learning Exercise (30 min)**

```python
# AWS Cost Tracker
costs = {
    "EC2": 500,
    "RDS": 300,
    "S3": 50,
    "CloudFront": 25,
    "Lambda": 10
}

print("=== AWS Monthly Cost Report ===")

total = 0
for service, cost in costs.items():
    print(f"{service:15} ${cost:6.2f}")
    total += cost

print("-" * 25)
print(f"{'Total':15} ${total:6.2f}")

# Find most expensive
most_expensive = max(costs, key=costs.get)
print(f"\nMost expensive: {most_expensive} (${costs[most_expensive]})")
```

### **Practice Exercises (60 min)**

**Exercise 4.1: Service Inventory Manager**

```python
# Create a dictionary of services and their status:
services = {
    "api": "running",
    "auth": "stopped",
    "database": "running",
    "cache": "running",
    "queue": "stopped"
}

# Tasks:
# 1. Print all services and their status
# 2. Count how many are "running" vs "stopped"
# 3. Print list of stopped services
# 4. Ask user for a service name and new status, update the dictionary
# 5. Print updated inventory
```

**Exercise 4.2: Kubernetes Pod Analyzer**

```python
# Given list of pod data:
pods = [
    {"name": "api-1", "status": "Running", "cpu": 0.5, "memory": 512},
    {"name": "api-2", "status": "Running", "cpu": 0.8, "memory": 600},
    {"name": "auth-1", "status": "Failed", "cpu": 0.0, "memory": 0},
    {"name": "db-1", "status": "Running", "cpu": 1.2, "memory": 2048},
    {"name": "cache-1", "status": "Pending", "cpu": 0.0, "memory": 0}
]

# Calculate:
# 1. How many pods in each status (Running, Failed, Pending)
# 2. Total CPU usage across all Running pods
# 3. Total memory usage across all Running pods
# 4. List of pods using > 1.0 CPU
# 5. Average memory per Running pod
```

**Exercise 4.3: Terraform State Analyzer**

```python
# Terraform state (simplified):
state = {
    "resources": [
        {"type": "aws_instance", "name": "web-1", "cost": 50},
        {"type": "aws_instance", "name": "web-2", "cost": 50},
        {"type": "aws_db_instance", "name": "main", "cost": 100},
        {"type": "aws_s3_bucket", "name": "assets", "cost": 10},
        {"type": "aws_lb", "name": "main", "cost": 25}
    ]
}

# Tasks:
# 1. Count resources by type (how many aws_instance, aws_db_instance, etc.)
# 2. Calculate total infrastructure cost
# 3. Find most expensive single resource
# 4. List all resource names of type "aws_instance"
```

**Exercise 4.4: Health Check Results Tracker**

```python
# Track multiple health checks over time:
checks = [
    {"timestamp": "10:00", "service": "api", "status": 200},
    {"timestamp": "10:01", "service": "api", "status": 200},
    {"timestamp": "10:02", "service": "api", "status": 500},
    {"timestamp": "10:00", "service": "auth", "status": 200},
    {"timestamp": "10:01", "service": "auth", "status": 200},
    {"timestamp": "10:02", "service": "auth", "status": 200}
]

# Calculate per-service statistics:
# - Total checks per service
# - Success rate per service (200 responses / total)
# - List of all failed checks (status != 200)
# Print summary for each service
```

### **Review Checklist (30 min)**

- [ ] I can create and manipulate lists
- [ ] I can access list items by index
- [ ] I can append, remove, and iterate over lists
- [ ] I can create and use dictionaries
- [ ] I can access, add, update, and remove dictionary keys
- [ ] I can iterate over dictionaries with .items()
- [ ] I can work with lists of dictionaries

-----

## Day 5: Functions

### **Core Concepts**

**Basic Functions:**

```python
# Define function
def greet(name):
    print(f"Hello, {name}")

# Call function
greet("Alice")  # Prints: Hello, Alice
```

**Return Values:**

```python
def add(a, b):
    result = a + b
    return result

# Capture return value
sum = add(5, 3)
print(sum)  # 8

# Multiple returns
def get_stats(numbers):
    total = sum(numbers)
    average = total / len(numbers)
    return total, average

total, avg = get_stats([10, 20, 30])
```

**Default Parameters:**

```python
def check_health(url, timeout=5):
    print(f"Checking {url} with {timeout}s timeout")

check_health("api.example.com")           # Uses default timeout=5
check_health("api.example.com", timeout=10)  # Custom timeout
```

**Type Hints (optional but good practice):**

```python
def calculate_cost(instances: int, price: float) -> float:
    return instances * price

# Makes code more readable and catches bugs
```

**Docstrings:**

```python
def classify_status(code: int) -> str:
    """
    Classify HTTP status code.
    
    Args:
        code: HTTP status code (e.g., 200, 404, 500)
    
    Returns:
        Classification string: "success", "client_error", or "server_error"
    """
    if 200 <= code < 300:
        return "success"
    elif 400 <= code < 500:
        return "client_error"
    elif 500 <= code < 600:
        return "server_error"
    else:
        return "unknown"
```

### **Learning Exercise (30 min)**

```python
# Service health functions

def check_service(name: str, status_code: int) -> str:
    """Check if service is UP or DOWN based on status code."""
    if status_code == 200:
        return "UP"
    else:
        return "DOWN"

def calculate_uptime(checks: list) -> float:
    """Calculate uptime percentage from list of status codes."""
    if not checks:
        return 0.0
    
    successful = sum(1 for code in checks if code == 200)
    return (successful / len(checks)) * 100

# Usage
api_status = check_service("api", 200)
print(f"API is {api_status}")

api_checks = [200, 200, 500, 200, 200]
uptime = calculate_uptime(api_checks)
print(f"API uptime: {uptime:.1f}%")
```

### **Practice Exercises (60 min)**

**Exercise 5.1: HTTP Status Classifier**

```python
def classify_status(code: int) -> str:
    """
    Classify HTTP status code into category.
    
    Returns: "success", "redirect", "client_error", "server_error", or "unknown"
    """
    # Your implementation
    pass

# Test cases
print(classify_status(200))  # Should print: success
print(classify_status(301))  # Should print: redirect
print(classify_status(404))  # Should print: client_error
print(classify_status(500))  # Should print: server_error
```

**Exercise 5.2: Kubernetes Resource Calculator**

```python
def calculate_pod_resources(cpu_per_pod: float, memory_per_pod: int, 
                           num_pods: int) -> tuple:
    """
    Calculate total cluster resources needed.
    
    Args:
        cpu_per_pod: CPU cores per pod (e.g., 0.5)
        memory_per_pod: Memory per pod in MB (e.g., 512)
        num_pods: Number of pods
    
    Returns:
        Tuple of (total_cpu, total_memory_gb)
    """
    # Your implementation
    # Remember to convert memory from MB to GB
    pass

# Test
cpu, mem_gb = calculate_pod_resources(0.5, 512, 10)
print(f"Total: {cpu} CPU cores, {mem_gb} GB memory")
```

**Exercise 5.3: Terraform Cost Estimator**

```python
def estimate_resource_cost(resource_type: str, count: int = 1) -> float:
    """
    Estimate monthly cost for AWS resources.
    
    Args:
        resource_type: "ec2", "rds", "s3", "alb"
        count: Number of resources
    
    Returns:
        Estimated monthly cost in dollars
    
    Prices:
        - ec2: $50/month
        - rds: $100/month
        - s3: $10/month
        - alb: $25/month
        - unknown: $0
    """
    # Your implementation
    pass

# Test
print(estimate_resource_cost("ec2", 5))   # Should print: 250.0
print(estimate_resource_cost("rds"))      # Should print: 100.0
print(estimate_resource_cost("unknown"))  # Should print: 0.0
```

**Exercise 5.4: Service Health Checker**

```python
def check_service_health(service_name: str, status_code: int, 
                        response_time_ms: int) -> dict:
    """
    Comprehensive service health check.
    
    Args:
        service_name: Name of the service
        status_code: HTTP response code
        response_time_ms: Response time in milliseconds
    
    Returns:
        Dictionary with keys:
        - "service": service name
        - "healthy": True if 200 status and response < 1000ms
        - "status": "healthy", "slow", or "down"
        - "details": Human-readable message
    """
    # Your implementation
    # A service is:
    # - "healthy" if status 200 and response < 1000ms
    # - "slow" if status 200 but response >= 1000ms
    # - "down" if status != 200
    pass

# Test
result = check_service_health("api", 200, 500)
print(result)
# Should print: {"service": "api", "healthy": True, "status": "healthy", ...}

result = check_service_health("auth", 200, 1500)
print(result)
# Should print: {"service": "auth", "healthy": False, "status": "slow", ...}

result = check_service_health("db", 500, 100)
print(result)
# Should print: {"service": "db", "healthy": False, "status": "down", ...}
```

### **Review Checklist (30 min)**

- [ ] I can define functions with parameters
- [ ] I can use return to send back values
- [ ] I can set default parameter values
- [ ] I can return multiple values as a tuple
- [ ] I can write docstrings to document functions
- [ ] I understand when to use functions (reusable logic)

-----

## Day 6: File I/O (Reading and Writing Files)

### **Core Concepts**

**Reading Files:**

```python
# Read entire file at once
with open("services.txt", "r") as file:
    content = file.read()
    print(content)

# Read line by line (better for large files)
with open("services.txt", "r") as file:
    for line in file:
        line = line.strip()  # Remove \n and whitespace
        print(line)

# Read all lines into list
with open("services.txt", "r") as file:
    lines = file.readlines()  # Returns list with \n included
    lines = [line.strip() for line in lines]  # Clean up
```

**Writing Files:**

```python
# Write (overwrites existing file)
with open("output.txt", "w") as file:
    file.write("First line\n")
    file.write("Second line\n")

# Append (adds to end of existing file)
with open("log.txt", "a") as file:
    file.write("New log entry\n")

# Write list of lines
lines = ["line 1", "line 2", "line 3"]
with open("output.txt", "w") as file:
    for line in lines:
        file.write(line + "\n")
```

**File Paths:**

```python
import os

# Check if file exists
if os.path.exists("services.txt"):
    print("File exists")

# Get absolute path
abs_path = os.path.abspath("services.txt")

# Join paths (works on Windows and Linux)
filepath = os.path.join("config", "dev", "settings.txt")
```

**Why use `with` statement:**

```python
# Good - file closes automatically even if error occurs
with open("file.txt", "r") as file:
    data = file.read()

# Bad - must remember to close, won't close if error
file = open("file.txt", "r")
data = file.read()
file.close()  # Might not execute if error above
```

### **Learning Exercise (30 min)**

Create `services.txt`:

```
api.example.com
auth.example.com
payments.example.com
database.example.com
```

Then run:

```python
# Service list reader
print("=== Service Health Check ===")

with open("services.txt", "r") as file:
    services = [line.strip() for line in file if line.strip()]

results = []
for service in services:
    # Simulate health check
    status = input(f"Status of {service} (up/down): ")
    results.append(f"{service}: {status}")

# Write results to file
with open("health_report.txt", "w") as file:
    file.write("=== Health Check Report ===\n")
    for result in results:
        file.write(result + "\n")

print("Report saved to health_report.txt")
```

### **Practice Exercises (60 min)**

**Exercise 6.1: Service URL Reader**

```python
# Create a file called 'services.txt' with these lines:
# https://api.example.com/health
# https://auth.example.com/health
# https://payments.example.com/health
#
# Write a script that:
# 1. Reads the file
# 2. Prints each URL
# 3. Counts total URLs
# 4. Prints: "Found X services"
```

**Exercise 6.2: Log Parser**

```python
# Create a file called 'app.log' with these lines:
# 2024-01-01 10:00:00 INFO Application started
# 2024-01-01 10:01:00 ERROR Connection failed
# 2024-01-01 10:02:00 INFO Request processed
# 2024-01-01 10:03:00 ERROR Timeout occurred
# 2024-01-01 10:04:00 INFO Request processed
# 2024-01-01 10:05:00 ERROR Database unavailable
#
# Write a script that:
# 1. Reads the log file
# 2. Counts ERROR lines
# 3. Prints all ERROR lines
# 4. Prints: "Found X errors out of Y total lines"
```

**Exercise 6.3: Cost Report Generator**

```python
# Create 'resources.txt' with:
# ec2,web-server-1
# ec2,web-server-2
# rds,database-main
# s3,assets-bucket
# s3,logs-bucket
# alb,load-balancer-main
#
# Costs per resource type:
# ec2: $50/month, rds: $100/month, s3: $10/month, alb: $25/month
#
# Write a script that:
# 1. Reads resources.txt
# 2. Calculates cost for each resource
# 3. Writes report to 'cost_report.txt' with format:
#    Resource Type | Resource Name | Monthly Cost
#    ec2           | web-server-1  | $50.00
#    ...
#    Total: $XXX.00
```

**Exercise 6.4: Configuration File Manager**

```python
# Write a script that:
# 1. Asks user for environment: dev, stage, or prod
# 2. Creates a config file named '{env}_config.txt'
# 3. Asks user for:
#    - Database host
#    - Database port
#    - API key
# 4. Writes these to the config file in format:
#    DATABASE_HOST=user_input
#    DATABASE_PORT=user_input
#    API_KEY=user_input
# 5. Confirms: "Configuration saved to {env}_config.txt"
#
# Then write another part that READS the config file and prints each setting
```

### **Review Checklist (30 min)**

- [ ] I can open and read files with `with open(...) as file:`
- [ ] I can read files line by line
- [ ] I can write to files (overwrite and append)
- [ ] I understand why `with` is important (automatic cleanup)
- [ ] I can check if a file exists before reading
- [ ] I can process file contents (strip whitespace, skip empty lines)

-----

## Day 7: Error Handling (try/except)

### **Core Concepts**

**Basic Exception Handling:**

```python
try:
    # Code that might fail
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# Program continues after except block
print("Program continues...")
```

**Multiple Exception Types:**

```python
try:
    file = open("missing.txt", "r")
    content = file.read()
except FileNotFoundError:
    print("File does not exist")
except PermissionError:
    print("No permission to read file")
except Exception as e:
    print(f"Unexpected error: {e}")
```

**Finally Block:**

```python
try:
    file = open("data.txt", "r")
    data = file.read()
except FileNotFoundError:
    print("File not found")
finally:
    # Always runs, even if exception occurred
    # Good for cleanup (close files, connections, etc.)
    if 'file' in locals():
        file.close()
```

**Raising Exceptions:**

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Divisor cannot be zero")
    return a / b

try:
    result = divide(10, 0)
except ValueError as e:
    print(f"Error: {e}")
```

**Common Exceptions in Platform Engineering:**

```python
import requests

try:
    response = requests.get("https://api.example.com", timeout=5)
except requests.Timeout:
    print("Request timed out")
except requests.ConnectionError:
    print("Cannot connect to server")
except requests.HTTPError:
    print("HTTP error occurred")
except Exception as e:
    print(f"Unexpected error: {e}")
```

### **Learning Exercise (30 min)**

```python
# Safe file reader with error handling

def read_service_file(filepath):
    """
    Safely read service list from file.
    Returns list of service URLs or empty list if error.
    """
    try:
        with open(filepath, "r") as file:
            services = [line.strip() for line in file if line.strip()]
            return services
    except FileNotFoundError:
        print(f"Error: File '{filepath}' not found")
        return []
    except PermissionError:
        print(f"Error: No permission to read '{filepath}'")
        return []
    except Exception as e:
        print(f"Unexpected error reading file: {e}")
        return []

# Usage
services = read_service_file("services.txt")
if services:
    print(f"Loaded {len(services)} services")
    for service in services:
        print(f"  - {service}")
else:
    print("No services loaded")
```

### **Practice Exercises (60 min)**

**Exercise 7.1: Safe Number Converter**

```python
def safe_int_convert(value: str) -> int:
    """
    Convert string to integer safely.
    
    Args:
        value: String to convert (e.g., "42", "abc")
    
    Returns:
        Integer value if conversion successful, 0 if failed
    
    Prints error message if conversion fails.
    """
    # Your implementation using try/except
    pass

# Test cases
print(safe_int_convert("42"))      # Should return: 42
print(safe_int_convert("abc"))     # Should return: 0 and print error
print(safe_int_convert("3.14"))    # Should return: 0 and print error
```

**Exercise 7.2: Safe File Writer**

```python
def write_health_report(filename: str, services: dict) -> bool:
    """
    Write service health report to file.
    
    Args:
        filename: Output file path
        services: Dictionary of {service_name: status}
    
    Returns:
        True if write successful, False otherwise
    
    Handle potential errors:
    - PermissionError (can't write to location)
    - IOError (disk full, etc.)
    """
    # Your implementation
    pass

# Test
services = {"api": "UP", "auth": "DOWN", "db": "UP"}
success = write_health_report("report.txt", services)
if success:
    print("Report written successfully")
else:
    print("Failed to write report")
```

**Exercise 7.3: Configuration File Reader**

```python
def load_config(env: str) -> dict:
    """
    Load configuration from {env}_config.txt file.
    
    File format:
        DATABASE_HOST=localhost
        DATABASE_PORT=5432
        API_KEY=secret123
    
    Args:
        env: Environment name (dev, stage, prod)
    
    Returns:
        Dictionary of config key-value pairs, or empty dict if error
    
    Handle:
    - FileNotFoundError
    - Malformed lines (missing =)
    """
    # Your implementation
    # Read file line by line
    # Split each line on '=' to get key and value
    # Handle errors gracefully
    pass

# Test
config = load_config("dev")
if config:
    print(f"Database: {config.get('DATABASE_HOST', 'not set')}")
```

**Exercise 7.4: HTTP Request with Retry**

```python
def check_service_with_retry(url: str, max_retries: int = 3) -> dict:
    """
    Check service health with retry logic.
    
    Args:
        url: Service health endpoint
        max_retries: Number of retry attempts
    
    Returns:
        Dictionary: {"status": status_code or error, "attempts": count}
    
    Note: For now, SIMULATE the request since you might not have
    the requests library installed. Ask user for status code.
    
    Handle errors:
    - Timeout (simulate by user typing "timeout")
    - Connection error (simulate by user typing "error")
    - Success (user types "200")
    
    Retry on timeout/error, stop on success.
    """
    # Your implementation
    # For each attempt:
    #   - Print f"Attempt {attempt}/{max_retries}..."
    #   - Ask user for result: "200", "timeout", or "error"
    #   - If "200", return success
    #   - If error and attempts remain, retry after 1 second
    #   - If max attempts reached, return failure
    pass

# Test
result = check_service_with_retry("api.example.com", max_retries=3)
print(f"Final status: {result}")
```

### **Review Checklist (30 min)**

- [ ] I can use try/except to handle errors
- [ ] I can catch specific exception types
- [ ] I understand when to use finally for cleanup
- [ ] I can handle file-related errors (FileNotFoundError, PermissionError)
- [ ] I can make functions fail gracefully (return default values, print errors)
- [ ] I understand why error handling is critical in production scripts

-----

# Week 1 Review: Weekend Integration Exercise

**Goal:** Combine all Week 1 concepts into one complete tool.

## **Project: Service Health Monitor (Complete Implementation)**

Build the script from the interview question that you couldn’t explain:

### **Requirements:**

1. Read service URLs from `services.txt` (one per line)
1. Check each URL’s “health” (simulate with user input)
1. Print summary showing which services are UP (200) and which are DOWN
1. Return exit code 1 if any service is DOWN, exit code 0 if all UP
1. Handle all errors gracefully (file not found, invalid input, etc.)

### **Structure:**

```python
#!/usr/bin/env python3
"""
Service Health Monitor
Reads service URLs from file and checks their health status.
"""

def read_services(filepath: str) -> list:
    """Read service URLs from file."""
    # Your implementation
    pass

def check_service(url: str) -> int:
    """
    Check service health.
    For now, asks user for status code.
    Later we'll use real HTTP requests.
    """
    # Your implementation
    pass

def print_summary(results: list) -> None:
    """Print formatted health check summary."""
    # Your implementation
    pass

def any_service_down(results: list) -> bool:
    """Check if any service returned non-200 status."""
    # Your implementation
    pass

def main():
    """Main entry point."""
    # Your implementation
    # 1. Read services from file
    # 2. Check each service
    # 3. Store results
    # 4. Print summary
    # 5. Exit with appropriate code
    pass

if __name__ == "__main__":
    main()
```

### **Expected Output:**

```
=== Service Health Check ===
Checking api.example.com... Status: 200 ✅ UP
Checking auth.example.com... Status: 500 ❌ DOWN
Checking db.example.com... Status: 200 ✅ UP

========================================
Summary: 2 UP, 1 DOWN
========================================

$ echo $?
1
```

**Time Budget:** 2-3 hours

**Success Criteria:**

- [ ] Script reads from file
- [ ] Script handles missing file gracefully
- [ ] Script checks each service
- [ ] Script prints formatted summary
- [ ] Script exits with correct code
- [ ] Code is organized into functions
- [ ] Functions have docstrings
- [ ] All errors are handled

**If you can build this from scratch without looking at references, you’re ready for Week 2.**

-----

# Week 2: Platform Scripting Patterns

**Goal:** Apply Week 1 fundamentals to real platform engineering tasks.

**Format:** Learn pattern (30 min) → Build tool (90 min)

-----

## Day 8: HTTP Requests with `requests` Library

### **Setup:**

```bash
pip install requests
```

### **Core Concepts:**

**Basic Requests:**

```python
import requests

# GET request
response = requests.get("https://api.example.com/health")

print(response.status_code)  # 200, 404, 500, etc.
print(response.text)         # Response body as string
print(response.json())       # Parse JSON response

# Request with timeout
response = requests.get(url, timeout=5)  # Timeout after 5 seconds

# Request with headers
headers = {
    "Authorization": "Bearer token123",
    "Content-Type": "application/json"
}
response = requests.get(url, headers=headers)

# POST request
data = {"name": "test", "value": 123}
response = requests.post(url, json=data)
```

**Error Handling:**

```python
try:
    response = requests.get(url, timeout=5)
    response.raise_for_status()  # Raises HTTPError for 4xx/5xx
    
except requests.Timeout:
    print("Request timed out")
except requests.ConnectionError:
    print("Cannot connect to server")
except requests.HTTPError as e:
    print(f"HTTP error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

### **Project: Real HTTP Health Checker (90 min)**

Upgrade your Weekend Project to use REAL HTTP requests:

```python
import requests

def check_service(url: str, timeout: int = 5) -> dict:
    """
    Check service health via HTTP.
    
    Returns:
        {"url": url, "status": status_code or error_string}
    """
    try:
        response = requests.get(url, timeout=timeout)
        return {"url": url, "status": response.status_code}
    except requests.Timeout:
        return {"url": url, "status": "TIMEOUT"}
    except requests.ConnectionError:
        return {"url": url, "status": "CONNECTION_FAILED"}
    except Exception as e:
        return {"url": url, "status": f"ERROR: {type(e).__name__}"}

# Now your health checker uses real HTTP instead of user input!
```

**Test with real URLs:**

```
# services.txt
https://httpstat.us/200
https://httpstat.us/500
https://httpstat.us/404
https://example.com/nonexistent
```

-----

## Day 9: Working with JSON (API Responses & Config Files)

### **Core Concepts:**

**Parsing JSON:**

```python
import json

# Parse JSON string
json_str = '{"name": "api", "status": "running", "port": 8080}'
data = json.loads(json_str)

print(data["name"])    # "api"
print(data["port"])    # 8080

# Convert Python dict to JSON string
config = {"database": "postgres", "port": 5432}
json_str = json.dumps(config)

# Pretty print JSON
print(json.dumps(config, indent=2))
```

**Reading/Writing JSON Files:**

```python
# Read JSON file
with open("config.json", "r") as file:
    config = json.load(file)

# Write JSON file
data = {"services": ["api", "auth"], "version": "1.0"}
with open("output.json", "w") as file:
    json.dump(data, file, indent=2)
```

**Working with API JSON Responses:**

```python
import requests

response = requests.get("https://api.github.com/repos/python/cpython")
repo_data = response.json()

print(f"Repository: {repo_data['name']}")
print(f"Stars: {repo_data['stargazers_count']}")
print(f"Language: {repo_data['language']}")
```

### **Project: Kubernetes Pod Status Monitor (90 min)**

```python
"""
Simulate parsing Kubernetes API JSON response.

In reality, you'd call: kubectl get pods -o json
For this exercise, we'll use a sample JSON file.
"""

import json

# Create sample_pods.json:
sample_data = {
    "items": [
        {"metadata": {"name": "api-pod-1"}, "status": {"phase": "Running"}},
        {"metadata": {"name": "auth-pod-2"}, "status": {"phase": "Failed"}},
        {"metadata": {"name": "db-pod-3"}, "status": {"phase": "Running"}},
        {"metadata": {"name": "cache-pod-4"}, "status": {"phase": "Pending"}}
    ]
}

with open("sample_pods.json", "w") as f:
    json.dump(sample_data, f, indent=2)

# Your task: Write script that:
# 1. Reads sample_pods.json
# 2. Parses the JSON
# 3. Counts pods by status (Running, Failed, Pending)
# 4. Lists failed pods
# 5. Calculates success rate (Running / Total)
# 6. Prints formatted report
```

-----

## Day 10: Running System Commands (subprocess)

### **Core Concepts:**

**Basic Command Execution:**

```python
import subprocess

# Run command, capture output
result = subprocess.run(
    ["ls", "-l"],
    capture_output=True,
    text=True
)

print(result.stdout)      # Command output
print(result.returncode)  # Exit code (0 = success)
print(result.stderr)      # Error output

# Check if successful
if result.returncode == 0:
    print("Command succeeded")
else:
    print(f"Command failed: {result.stderr}")
```

**Running Shell Commands:**

```python
# With shell=True (be careful with user input!)
result = subprocess.run(
    "kubectl get pods | grep Running",
    shell=True,
    capture_output=True,
    text=True
)

# Safer: use list of arguments
result = subprocess.run(
    ["kubectl", "get", "pods", "-o", "json"],
    capture_output=True,
    text=True
)
```

**Timeout and Error Handling:**

```python
try:
    result = subprocess.run(
        ["long-running-command"],
        timeout=10,  # Kill after 10 seconds
        capture_output=True,
        text=True
    )
except subprocess.TimeoutExpired:
    print("Command timed out")
except subprocess.CalledProcessError as e:
    print(f"Command failed with exit code {e.returncode}")
```

### **Project: Terraform Plan Analyzer (90 min)**

```python
"""
Run terraform plan and analyze the output.

Tasks:
1. Execute: terraform plan -no-color
2. Capture output
3. Parse output to count:
   - Resources to create (lines with "+ resource")
   - Resources to destroy (lines with "- resource")
   - Resources to modify (lines with "~ resource")
4. Calculate risk score:
   - Destroy = 10 points each
   - Modify = 2 points each
   - Create = 1 point each
5. Print risk assessment:
   - 0-5: Low risk
   - 6-20: Medium risk
   - 21+: High risk - approval required
6. Return exit code 1 if high risk, 0 otherwise

Note: For testing without Terraform, create a sample plan output file:
terraform_plan_sample.txt with lines like:
  + aws_instance.web
  - aws_db_instance.old
  ~ aws_security_group.main
"""
```

-----

## Day 11-12: Complete Tool Build - Log Error Monitor

**This is the tool that failed you in Round 2. Now you’ll build it from scratch.**

### **Day 11: Basic Version (Simplified)**

Build log monitor that:

1. Reads `app.log` file
1. Parses each line for timestamp and error marker
1. Counts errors in last 60 seconds (sliding window)
1. Prints alert if > 5 errors in window

**Simplified approach (no real file tailing yet):**

```python
"""
Log Error Monitor - Basic Version

Sample app.log format:
2024-01-01 10:00:00 INFO Request processed
2024-01-01 10:00:15 ERROR Connection failed
2024-01-01 10:00:30 ERROR Timeout
...

Tasks:
1. Read app.log file
2. Parse each line to extract:
   - Timestamp
   - Log level (INFO, ERROR, etc.)
3. Filter ERROR lines
4. For each ERROR, check if it's within last 60 seconds of file end
5. If >5 errors in that window, print ALERT
"""

from datetime import datetime, timedelta

def parse_log_line(line: str) -> dict:
    """
    Parse log line into components.
    
    Returns:
        {"timestamp": datetime, "level": str, "message": str}
        or None if line is invalid
    """
    # Your implementation
    pass

def count_recent_errors(log_lines: list, window_seconds: int = 60) -> int:
    """Count errors within the last window_seconds."""
    # Your implementation
    pass

def main():
    # Read log file
    # Parse all lines
    # Find most recent timestamp
    # Count errors in window before that timestamp
    # Print alert if threshold exceeded
    pass
```

### **Day 12: Enhanced Version (With Monitoring Loop)**

Add continuous monitoring:

1. Check log file every 5 seconds
1. Track new errors as they appear
1. Maintain sliding window of errors
1. Alert when threshold breached
1. Cooldown period (don’t spam alerts)

-----

## Day 13-14: Complete Tool Build - Secret Rotation Orchestrator

Build tool that orchestrates secret rotation:

### **Simplified Version:**

```python
"""
Secret Rotation Orchestrator

Simulates rotating a database password across microservices.

Steps:
1. Generate new password
2. Update Vault (simulate)
3. Update Kubernetes secrets in 3 namespaces (simulate)
4. Restart pods gradually (simulate)
5. Verify health after each restart
6. If any step fails, rollback

This is a SIMULATION. In reality, you'd call:
- Vault API
- Kubernetes API
- Check actual health endpoints

For this exercise, we'll:
- Print what we would do
- Ask user if step succeeded
- Implement retry logic
- Implement rollback on failure
"""

def generate_password(length: int = 16) -> str:
    """Generate random password."""
    pass

def update_vault(secret_name: str, new_value: str) -> bool:
    """Simulate Vault update."""
    print(f"Updating Vault secret: {secret_name}")
    # Ask user if succeeded
    pass

def update_k8s_secret(namespace: str, secret_name: str, value: str) -> bool:
    """Simulate K8s secret update."""
    pass

def restart_pods(namespace: str, max_at_once: int = 2) -> bool:
    """Simulate gradual pod restart."""
    pass

def verify_health(namespace: str) -> bool:
    """Simulate health check."""
    pass

def rollback_secret(namespace: str, old_value: str) -> None:
    """Rollback to previous secret value."""
    pass

def main():
    """Orchestrate full rotation."""
    # 1. Generate new password
    # 2. Update Vault (with retry)
    # 3. For each namespace:
    #    - Update secret
    #    - Restart pods gradually
    #    - Verify health
    #    - If failure, rollback all previous namespaces
    # 4. Print final status
    pass
```

-----

# Week 3: System Design Mental Models

**You’re now ready for high-level architecture patterns.**

Now that you have programming fluency, you can reason about the logic inside platform systems. This week focuses on the mental models that show up in every platform engineering interview.

**Format:** 30 min pattern study + 30 min architecture design + 30 min verbal explanation

-----

## Day 15: State Machines (Lifecycle Management)

### **Core Pattern**

Every deployment, health check, and circuit breaker is a state machine.

**Mental Model:**

```
State = what you remember between events
Event = something that happened (API call, timeout, metric threshold)
Transition = state changes based on event
Action = side effect (alert, deploy, rollback)
```

### **Worked Example: Circuit Breaker**

```
States: CLOSED, OPEN, HALF_OPEN

CLOSED (normal operation):
  - success → stay CLOSED
  - failure → increment failure_count
  - failure_count > threshold → transition to OPEN, start timer

OPEN (blocking requests):
  - any request → immediate failure, don't call service
  - timeout_expired → transition to HALF_OPEN

HALF_OPEN (testing recovery):
  - success → transition to CLOSED, reset failure_count
  - failure → transition to OPEN, restart timer
```

**Why this matters:** Without state tracking, you’d retry indefinitely or give up too early. The state machine prevents thundering herd and allows graceful recovery.

### **Study Exercise (30 min)**

Read this deployment gate state machine:

```
States: ANALYZING, WAITING_APPROVAL, APPLYING, COMPLETE, FAILED

ANALYZING:
  - plan_parsed successfully → WAITING_APPROVAL
  - plan_parse_failed → FAILED

WAITING_APPROVAL:
  - approval_granted → APPLYING
  - approval_denied → FAILED
  - approval_timeout → FAILED

APPLYING:
  - apply_success → COMPLETE
  - apply_failed → FAILED

From FAILED:
  - manual_retry → ANALYZING
```

Draw this as a diagram on paper with boxes (states) and arrows (transitions).

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Design a state machine for pod health checking.

**Requirements:**

- Pod starts in STARTING state
- After 3 consecutive successful health checks → READY
- After 3 consecutive failures from READY → DEGRADED
- After 5 consecutive failures from DEGRADED → FAILING
- If pod recovers (3 successes from DEGRADED) → READY

**Deliverable:** State diagram with all transitions labeled.

### **Verbal Explanation (30 min - Record Yourself)**

Explain your pod health checker state machine out loud as if in an interview:

- What are the states and why?
- What events trigger transitions?
- Why 3 successes to become READY (not 1)?
- Why separate DEGRADED and FAILING states?
- What would you track in each state (consecutive failure count, timestamp)?

**Play back recording. Does it make sense?**

-----

## Day 16: Event-Driven Architecture (Decoupling)

### **Core Pattern**

Producer emits events. Consumer processes asynchronously. Neither blocks the other.

**Mental Model:**

```
Synchronous: A calls B, A waits for B to finish (blocked)
Asynchronous: A publishes event, returns immediately. B processes later.

When to use async:
- B's latency shouldn't block A's response (Slack alerting)
- B might be down, A should still work (monitoring sidecar)
- Multiple consumers need same event (EventBridge → Lambda + SNS + Kinesis)
```

### **Worked Example: EIP Chat Error Handler (Fixed)**

**Current (Bad):**

```
User → FastAPI → LLM fails → POST to Slack (blocks 5s) → return 500 to user
Problem: User waits for Slack timeout
```

**Fixed (Good):**

```
User → FastAPI → LLM fails → publish to EventBridge → return 500 immediately (10ms)
        (separately) EventBridge → Lambda → POST to Slack
```

**Why:** EventBridge is a durable queue. If Slack is down, the event stays in the queue and retries. User never waits.

### **Study Exercise (30 min)**

Compare these architectures for secret rotation:

**Synchronous:**

```
Vault rotates password
  → Call K8s API to update secret (blocks 2s)
  → Call restart API for each pod (blocks 30s total)
  → Call Slack to notify (blocks 5s)
  → Vault rotation completes after 37s
```

**Asynchronous:**

```
Vault rotates password
  → Publish "PasswordRotated" event to EventBridge
  → Vault rotation completes immediately

EventBridge routes event to:
  → Lambda 1: Update K8s secrets
  → Lambda 2: Restart pods gradually
  → Lambda 3: Send Slack notification
Each Lambda runs independently
```

**Question:** What if K8s API is down? In sync version, rotation fails. In async version?

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Terraform apply audit trail

**Requirements:**
When `terraform apply` runs, log to:

- S3 (for compliance retention)
- CloudWatch (for searching)
- Slack (for team notification)
- DataDog (for metrics)

**Design both:**

1. Synchronous version (terraform waits for all 4 writes)
1. Asynchronous version (terraform publishes event, returns immediately)

**Label:**

- Which components block terraform?
- What happens if Slack is down?
- How do you know all logging succeeded?

### **Verbal Explanation (30 min - Record Yourself)**

Explain your async design:

- Why EventBridge over direct Slack API call?
- What if EventBridge is down? (Terraform apply still succeeds — audit logs might be missed, but deploy isn’t blocked)
- How do you verify all 4 destinations received the event? (Dead letter queue, CloudWatch metrics on Lambda failures)
- Trade-off: Complexity vs reliability

-----

## Day 17: Failure Modes & Retries (Resilience)

### **Core Pattern**

Every external call can fail. You need retry logic, backoff, circuit breakers, timeouts.

**Mental Model:**

```
Transient failure: Network blip, rate limit → Retry succeeds
Permanent failure: Service down, 404 → Retry won't help

Retry strategies:
- Immediate: retry now (fast transient failures)
- Fixed delay: retry every 5s (simple but can overwhelm)
- Exponential backoff: 1s, 2s, 4s, 8s (best for rate limits)
- Jitter: add randomness (prevents thundering herd)

Always include:
- Max attempts (don't retry forever)
- Timeout per attempt (don't wait forever)
- Circuit breaker (stop trying if service is down)
```

### **Worked Example: Init Container Fetching Secrets**

**Scenario:** Pod needs secret from Vault before starting.

**Naive approach (bad):**

```python
secret = vault.read("database/password")  # No timeout, no retry
```

**What breaks:**

- Vault is slow (30s response) → Pod stuck in Init
- Vault returns 503 (overloaded) → Pod fails immediately
- Network blip (transient) → Pod fails, doesn’t retry

**Production approach (good):**

```python
def fetch_secret_with_retry(path, max_attempts=5):
    for attempt in range(max_attempts):
        try:
            secret = vault.read(path, timeout=5)
            return secret
        except Timeout:
            if attempt == max_attempts - 1:
                raise  # Final attempt, give up
            sleep_time = min(2 ** attempt, 30)  # Exponential backoff, max 30s
            sleep(sleep_time)
        except VaultDown:
            # Permanent failure, don't retry
            raise
```

**Retry schedule:** 0s → 2s → 4s → 8s → 16s (total ~30s before giving up)

### **Study Exercise (30 min)**

Analyze this ArgoCD sync with timeout:

```python
def wait_for_sync(app_name, timeout_seconds=300):
    start = time.time()
    
    while time.time() - start < timeout_seconds:
        status = argocd.get_app_status(app_name)
        
        if status == "Synced":
            return "SUCCESS"
        elif status == "Failed":
            return "FAILED"
        
        time.sleep(5)  # Poll every 5s
    
    return "TIMEOUT"
```

**Questions:**

- What if ArgoCD API is down? (get_app_status throws exception)
- What if it returns “Syncing” for 10 minutes? (We timeout after 5 min)
- What if it flaps between “Synced” and “Degraded”? (We might return success prematurely)

**How to fix?** Add retry logic to API call, require N consecutive “Synced” statuses, add circuit breaker.

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Slack alert delivery with retries

**Requirements:**

- POST to Slack webhook
- Slack might: be slow (2s response), rate limit (429), be down (503), timeout
- Don’t block the main application waiting for Slack
- Ensure alert eventually delivers

**Design:**

1. Retry strategy (how many attempts? what backoff?)
1. Timeout per attempt
1. Dead letter queue (where do failed alerts go after max retries?)
1. How do you know an alert was never delivered?

**Draw the flow with retry logic.**

### **Verbal Explanation (30 min)**

Explain your design decisions:

- Why exponential backoff over fixed delay?
- Why DLQ instead of retrying forever?
- What if Slack is down for 2 hours? (Alert sits in DLQ, ops team notified via different channel)
- How do you prevent alert storms? (Rate limiting, deduplication, cooldown)

-----

## Day 18: Resource Pools & Quotas (Multi-Tenancy)

### **Core Pattern**

Shared platforms need resource isolation to prevent noisy neighbors.

**Mental Model:**

```
Pool pattern:
- Pre-allocate limited resources (CPU, memory, IPs, connections)
- Enforce quotas per tenant
- Track usage for chargeback

Common pools:
- Database connections (max 100)
- Kubernetes ResourceQuota (10 CPU per namespace)
- VPC CIDR blocks (/24 per team)
- API rate limits (1000 req/hour per key)
```

### **Worked Example: Kubernetes ResourceQuota**

**Without quotas:**

```
Team A deploys 50 pods (5 CPU each) = 250 CPU
Team B tries to deploy → "Insufficient CPU"
Cluster has 256 CPU total, Team A consumed 97%
```

**With quotas:**

```yaml
# team-a-quota
cpu: "100"        # Max 100 CPU cores
memory: "200Gi"   # Max 200GB RAM
pods: "50"        # Max 50 pods
```

**Now:**

- Team A hits quota at 20 pods (100 CPU used)
- Team A’s 21st pod: “Forbidden: exceeded quota”
- Team B still has 156 CPU available
- Platform remains stable

### **Study Exercise (30 min)**

Analyze this connection pool exhaustion:

**Setup:**

- RDS instance: 100 max connections
- 20 microservices share this database
- Each service creates 10 connections at startup

**Math:** 20 services × 10 connections = 200 needed, only 100 available

**What happens:**

- First 10 services start successfully (100 connections used)
- Services 11-20 crash: “Connection refused”
- First 10 services work fine, unaware of problem
- On-call gets paged at 3am

**Solutions:**

1. Connection pooling (share connections across requests)
1. Quota per service (each gets max 5 connections)
1. Circuit breaker (service stops trying after N failures)

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Multi-tenant namespace quotas

**Requirements:**

- Platform supports 50 teams
- Each team gets a namespace
- Prevent any team from consuming >20% of cluster resources

**Design:**

1. What quotas do you set per namespace? (CPU, memory, pods, load balancers, persistent volumes)
1. Cluster has 500 CPU, 1TB RAM. What’s the per-namespace quota?
1. What happens when a team hits their quota?
1. How does a team request more quota? (Approval workflow?)

**Draw the quota enforcement architecture.**

### **Verbal Explanation (30 min)**

Walk through your design:

- Why set quotas below cluster capacity? (Leave headroom for system pods, surge capacity)
- Why limit pods, not just CPU? (Prevent IP exhaustion, scheduler overhead)
- How do you handle “VIP teams” that need more? (Separate namespace with higher quota, or dedicated cluster)
- How do you detect quota abuse? (Metrics on quota usage, alerts at 80% utilization)

-----

## Day 19: API Gateway Patterns (Control Plane Design)

### **Core Pattern**

Platform teams expose infrastructure via APIs, not raw Terraform/kubectl.

**Mental Model:**

```
User → API Gateway → Backend Service → Infrastructure (K8s, Terraform, Vault)

API Gateway provides:
- Authentication (who are you?)
- Authorization (what can you do?)
- Rate limiting (prevent abuse)
- Request validation (schema enforcement)
- Audit logging (compliance)
- Circuit breaking (protect backend)
```

### **Worked Example: Namespace Provisioning API**

**Without API Gateway (bad):**

```
User runs: kubectl create namespace my-app
Problems:
- No authentication (anyone can create)
- No quota enforcement (user creates 100 namespaces)
- No audit trail (who created what when?)
- No validation (user creates namespace "PROD" by mistake)
```

**With API Gateway (good):**

```
User → POST /namespaces {name: "my-app"} → API Gateway checks:
  ✓ JWT valid? (authentication)
  ✓ User in "developers" group? (authorization)
  ✓ User hasn't hit rate limit? (10 namespaces/hour)
  ✓ Namespace name matches pattern? (lowercase, no "prod")
  → Backend service creates namespace
  → Log to audit trail: {user: "alice", action: "create_namespace", ...}
```

### **Study Exercise (30 min)**

Compare API Gateway vs ALB for platform API:

|Feature           |API Gateway            |ALB                      |
|------------------|-----------------------|-------------------------|
|Authentication    |Built-in (JWT, Cognito)|Must implement yourself  |
|Rate limiting     |Built-in per key       |Must implement in backend|
|Request validation|JSON schema validation |Must implement yourself  |
|WebSocket         |✅ (v2 only)            |✅                        |
|Cost              |Pay per request        |Pay per hour             |
|Latency           |+10ms overhead         |+5ms overhead            |

**When to use API Gateway:**

- Need built-in auth/rate limiting
- Request validation important
- Don’t want to build these features

**When to use ALB:**

- WebSocket required (and using API Gateway v1)
- Want lower latency
- Already have auth/rate limiting in backend

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Platform provisioning API

**Endpoints:**

- POST /namespaces (create namespace with quotas)
- POST /databases (provision RDS instance)
- POST /secrets (create Vault secret)

**Design:**

1. Should this be sync or async? (User waits or gets job ID?)
1. What authentication method? (JWT from Okta, mTLS, API keys?)
1. What rate limits? (Per user? Per team? Per endpoint?)
1. How do you validate requests? (JSON schema? Custom logic?)
1. Where do you log audit trail? (CloudTrail, DynamoDB, both?)

**Draw the request flow from user to infrastructure.**

### **Verbal Explanation (30 min)**

Explain your design:

- Why async for database provisioning? (Takes 5 min, don’t block user)
- Why sync for secret creation? (Takes 100ms, user needs immediate confirmation)
- How do you prevent abuse? (Rate limiting: 10 databases/day, 100 secrets/hour)
- How do you handle approval workflows? (High-value resources require approval, store in DynamoDB, poll status)

-----

## Day 20: Secrets Management Architecture

### **Core Pattern**

Apps need secrets. Platform provides them securely without manual `kubectl create secret`.

**Mental Model:**

```
Secrets flow: Vault (source of truth) → K8s Secret → Pod

Delivery methods:
- Init container: Fetch at startup, write to file
- Sidecar: Continuously sync (handles rotation)
- External Secrets Operator: Controller watches Vault, updates K8s Secret

Rotation without downtime:
- Dual-password support (old + new valid for 5 min)
- Rolling restart (10% pods at a time)
- Health check before marking complete
```

### **Worked Example: Secret Rotation Without Downtime**

**Challenge:** 200 microservices use DB password. Password rotates every 30 days. Can’t restart all at once.

**Solution:**

**Step 1: Enable dual-password in database**

```sql
-- Both old and new passwords valid for 5 minutes
ALTER USER app_user WITH PASSWORD 'new_password_123';
-- Old password still works for 5 min transition window
```

**Step 2: Update K8s Secret**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
  # Annotation triggers rolling restart
  annotations:
    checksum: "abc123"  # Changes when secret updates
data:
  password: "new_password_123"
```

**Step 3: Rolling restart**

```yaml
spec:
  strategy:
    rollingUpdate:
      maxUnavailable: 10%  # Only 20 pods restart at a time
```

**Timeline:**

- T+0: Rotate password in Vault
- T+1: Update K8s Secret
- T+2: Rolling restart begins (20 pods)
- T+5: First batch using new password
- T+10: Second batch restarts
- T+30: All 200 pods restarted
- T+31: Disable old password

### **Study Exercise (30 min)**

Compare secret delivery methods:

**Init Container:**

```yaml
initContainers:
- name: fetch-secrets
  image: vault:1.12
  command: ["vault", "read", "database/password"]
  volumeMounts:
  - name: secrets
    mountPath: /secrets
```

**Pros:** Simple, no sidecar overhead  
**Cons:** No automatic rotation, pod must restart

**Sidecar (Vault Agent):**

```yaml
containers:
- name: vault-agent
  image: vault:1.12
  command: ["vault", "agent", "-config=/vault/config"]
```

**Pros:** Auto-rotation, no restart needed  
**Cons:** Extra container per pod, resource overhead

**External Secrets Operator:**

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-password
spec:
  secretStoreRef:
    name: vault
  target:
    name: db-credentials
  data:
  - secretKey: password
    remoteRef:
      key: database/password
```

**Pros:** Centralized, watches Vault, auto-updates K8s Secret  
**Cons:** Operator must be running, K8s Secret still needs pod restart

### **Design Exercise (30 min - Paper Only)**

**Scenario:** Zero-downtime database password rotation

**Requirements:**

- 50 microservices use PostgreSQL
- Password rotates monthly (compliance)
- Max 5 pods restart simultaneously (capacity constraint)
- No database connection errors during rotation

**Design:**

1. How do you enable dual-password support?
1. What triggers pod restart? (Secret annotation checksum)
1. How do you control restart rate? (maxUnavailable: 10%)
1. How do you verify health before continuing? (readiness probes)
1. What if a pod fails to restart? (Halt rollout, alert ops)

**Draw the rotation flow with timing.**

### **Verbal Explanation (30 min)**

Walk through the rotation:

- Why dual-password for 5 minutes? (Transition window — pods restart gradually, some still have old password cached)
- Why annotation checksum triggers restart? (K8s doesn’t auto-restart on Secret change by design — you must explicitly trigger)
- What if database rejects new password? (Rollback Secret to old value, investigate offline)
- How do you test this? (Staging environment, simulate rotation, verify zero errors)

-----

## Day 21: System Design Capstone

**Full Interview Scenario (90 min):**

Design a **CI/CD platform for 200 microservices**

### **Requirements:**

- 200 services across 20 teams
- Each service deploys 5-10x/day to dev, staging, prod
- Need: Fast feedback (<5 min build), progressive rollout, auto-rollback on errors
- Constraints: AWS only, Kubernetes for compute, GitOps for deploys

### **Your Task (On Paper):**

**Part 1: High-Level Architecture (30 min)**

Draw component diagram showing:

- Entry point (GitHub webhook triggers build)
- Build orchestration (GitHub Actions, Jenkins, CircleCI?)
- Artifact storage (ECR for Docker images)
- GitOps tool (ArgoCD, Flux?)
- Deployment pipeline stages (dev → staging → prod)
- Observability stack (metrics, logs, traces)

Label each component and data flow.

**Part 2: Design Decisions (30 min)**

For each decision, explain trade-offs:

**1. API Gateway vs ALB for platform APIs?**

- Considerations: Auth, rate limiting, cost, latency
- Decision: ?
- Rationale: ?

**2. Sync vs async namespace creation?**

- Sync: User waits for response (30s risk)
- Async: Return job ID, user polls status
- Decision: ?

**3. EventBridge vs direct API calls between platform services?**

- EventBridge: Decoupled, durable, multi-consumer
- Direct: Simple, fast, tightly coupled
- Decision: ?

**4. Single large EKS cluster vs multiple smaller clusters?**

- Single: Simpler ops, resource sharing
- Multi: Better isolation, blast radius containment
- Decision: ?

**5. Where do Terraform states live?**

- S3 bucket structure
- Locking mechanism
- Per-environment separation
- How to prevent dev state + prod vars catastrophe?

**Part 3: Failure Scenarios (20 min)**

How do you handle:

**1. GitHub is down**

- Can teams still deploy? (No, unless you have artifact cache)
- Mitigation: ?

**2. ArgoCD stuck syncing**

- How to detect? (Timeout on sync wait)
- How to alert? (CloudWatch alarm)
- Manual intervention needed? (Force sync, investigate)

**3. Build takes >30 minutes**

- Timeout and notification?
- Root cause? (Large image, slow tests, resource constraints)
- Fix: Cache layers, parallelize tests, bigger runners

**4. RDS primary fails during deployment**

- Impact on pipeline? (Deployment continues or fails?)
- Design decision: Pipeline shouldn’t depend on RDS if possible
- If needed for state: Retry logic, fallback to read replica

**Part 4: Security & Compliance (10 min)**

**1. Where do secrets live?**

- Vault for source of truth
- K8s Secrets for delivery
- How do pods access? (Init container, External Secrets Operator)

**2. Who can deploy to prod?**

- RBAC via Okta/SAML
- Only “platform-deploy” group
- Approval workflow for risky changes

**3. Audit trail**

- CloudTrail for AWS API calls
- K8s audit logs for kubectl
- Platform API logs in DynamoDB
- Immutable, retention 7 years

**4. Prevent privilege escalation**

- OPA Gatekeeper policies
- Block: privileged pods, hostPath, capabilities
- Enforce: Pod Security Standards

### **Deliverable:**

1. **Architecture diagram** (all components, data flows, failure paths)
1. **1-page design doc** (key decisions, trade-offs, open questions)
1. **State machine** for deployment lifecycle (PENDING → BUILDING → DEPLOYING → COMPLETE/FAILED)
1. **Security boundaries** diagram (network zones, IAM roles, secret access)

### **Verbal Explanation (30 min after you finish)**

Record yourself presenting this design as if to a hiring manager:

- Start with high-level architecture
- Walk through a deployment flow
- Explain key decisions and trade-offs
- Address failure scenarios
- Discuss security model

**Play it back. Would you hire this person?**

-----

# Week 3 Complete: You’re Ready

After completing Week 3, you should be able to:

✅ **Design platform systems** on whiteboard in 30 minutes  
✅ **Explain component choices** with clear trade-offs  
✅ **Trace failure paths** and propose mitigations  
✅ **Pseudocode implementation details** when asked  
✅ **Answer “what if X fails”** confidently  
✅ **Switch between high-level and low-level** fluidly

**Most importantly:** You won’t reach for Claude to generate code during interviews. You’ll design it yourself.

-----

-----

# Success Criteria: When Are You Ready?

## **After Week 1 (Programming Fundamentals)**

You should be able to, **on paper, in 15 minutes, without references**:

- [ ] Pseudocode a service health checker that reads URLs from file
- [ ] Pseudocode a log parser that counts errors
- [ ] Pseudocode a cost calculator that sums resource costs
- [ ] Explain what each line of your pseudocode does
- [ ] Identify where errors might occur and how to handle them

## **After Week 2 (Platform Scripting)**

You should be able to, **on computer, in 60 minutes, without copying code**:

- [ ] Build a working HTTP health checker
- [ ] Build a log error counter
- [ ] Build a JSON config parser
- [ ] Build a system command wrapper with error handling
- [ ] Explain every line of code you wrote

## **After Week 3 (System Design)**

You should be able to, **in an interview, with just whiteboard**:

- [ ] Design multi-region architecture with failover
- [ ] Design secret rotation without downtime
- [ ] Design CI/CD pipeline with progressive rollout
- [ ] Design observability for multi-tenant platform
- [ ] Explain failure modes and mitigation for any design

-----

# Daily Practice Template

## **Learning Phase (30 min)**

- Read concept section carefully
- Type out example code yourself (don’t copy-paste)
- Run examples, see output
- Break something, fix it
- Understand WHY, not just WHAT

## **Coding Phase (60 min)**

- Close all references
- Set timer
- Solve exercise from scratch
- Google is allowed for syntax only
- No copying code from anywhere
- If stuck >15 min, review concept, start over

## **Review Phase (30 min)**

- Compare your solution to reference (if provided)
- Identify what you struggled with
- Write down the pattern you missed
- Redo that exercise tomorrow as warm-up
- Update your “concepts I understand” checklist

-----

# Critical Rules

## **NO Claude/ChatGPT During Exercises**

You **must** write code yourself. Every line. Every function.

**Allowed:**

- Googling “python how to read file”
- Checking Python documentation
- Looking up syntax errors

**NOT Allowed:**

- “Claude, write a function to check service health”
- Copying entire code blocks from anywhere
- Using AI to debug your code
- Generating solutions with AI then studying them

**Why:** The interview failure happened because you generated code you couldn’t explain. The only way to fix this is to BUILD the muscle yourself.

## **If You Get Stuck**

1. **Reread the concept section** (5 min)
1. **Write pseudocode on paper** (10 min)
1. **Try to code just one function** (10 min)
1. **Still stuck? Take a break** (15 min)
1. **Come back and try different approach** (10 min)
1. **After 45 min, look at hint/example and try again**

Struggling is PART OF LEARNING. If it’s easy, you’re not learning.

## **Daily Minimum**

- **Can’t do 2 hours?** Do 1 hour. But do it EVERY day.
- **Missed a day?** Don’t skip ahead. Redo that day.
- **Still struggling with Day 3 concepts?** Stay on Day 3 until you get it.

**Consistency > Speed. You have 3 weeks before next interview.**

-----

# Resources

## **Python Learning**

- Official Python Tutorial: docs.python.org/3/tutorial
- Real Python (beginner articles): realpython.com
- Python documentation: docs.python.org/3/library

## **Practice Problems**

- HackerRank Python track (Easy only): hackerrank.com/domains/python
- Codewars (8 kyu problems): codewars.com
- Your own exercises in this plan

## **When Stuck on Syntax**

- Google: “python [what you want to do]”
- Stack Overflow: stackoverflow.com
- Python docs: docs.python.org

## **DON’T Use (During Learning)**

- Claude AI / ChatGPT for code generation
- Code completion tools that write entire functions
- Copy-pasting from tutorials

-----

# Weekly Check-ins

## **End of Week 1**

- [ ] Can I read and write files without looking up syntax?
- [ ] Can I write functions with return values?
- [ ] Can I iterate over lists and dictionaries?
- [ ] Can I handle errors with try/except?
- [ ] Can I build the Weekend Project from scratch?

**If NO to any:** Spend Week 2 Day 1 reviewing that concept.

## **End of Week 2**

- [ ] Can I make HTTP requests with error handling?
- [ ] Can I parse JSON responses?
- [ ] Can I run system commands and capture output?
- [ ] Can I build a complete health checker tool?
- [ ] Can I explain my code to someone else?

**If NO to any:** Don’t move to Week 3 yet.

## **End of Week 3**

- [ ] Can I pseudocode any platform pattern in 10 minutes?
- [ ] Can I design system architecture on whiteboard?
- [ ] Can I explain failure modes and trade-offs?
- [ ] Can I answer “what if X fails” without panicking?
- [ ] Am I ready to interview without generating code with AI?

**If YES to all:** You’re ready. Schedule that interview.

-----

# The Commitment

**This is a 3-week bootcamp. It requires discipline.**

**Your commitment:**

- 2 hours/day, every day
- No shortcuts
- No AI-generated code
- No skipping ahead
- Struggle through the exercises yourself

**Your reward:**

- Never fail an implementation question again
- Confidently pseudocode any platform tool
- Explain your own code fluently
- Pass Round 2 at your next opportunity

-----

# After 3 Weeks: Interview Ready

**You’ll walk into an interview and when they ask:**

> “Design a log monitoring system that alerts when error rate exceeds threshold”

**You’ll do this:**

1. **Pseudocode on whiteboard** (5 min):

```
error_window = queue()

while true:
    line = read_next_line()
    if is_error(line):
        error_window.append((now(), line))
    
    prune_old_entries(error_window, now() - 60)
    
    if len(error_window) > 5:
        send_alert()
```

1. **Walk through the logic** (3 min):
   “I’m using a queue to maintain a sliding window. On each log line, I check if it’s an error. If yes, append to the window with timestamp. Then I prune entries older than 60 seconds. If the remaining count exceeds threshold, I alert.”
1. **Address failure modes** (2 min):
   “If the log file rotates, I’d detect it by checking inode. If Slack is down, I’d log the failure and retry on next breach. If we get 1000 errors per second, the window would grow large — I’d add a max window size and alert on that too.”

**This is what you couldn’t do in Round 2. Now you can.**

-----

**Start tomorrow. Day 1. Variables and I/O. 2 hours. No exceptions.**

Good luck. You’ve got this.



::::::::::::::::::::::::::::::::::

# Platform Engineering Interview Study Plan

## 2-Week Program for High-Level Architecture Mastery

**Goal:** Build mental models for platform architecture discussions and system design interviews at companies like NVIDIA.

**Time Commitment:** 90 minutes/day  
**Format:** 30 min study + 30 min diagram + 30 min verbal explanation  
**Rules:** No code generation during study. Paper/whiteboard only. No AI assistance during exercises.

-----

## Week 1: Foundational Mental Models

### Day 1: State Machines (Lifecycle Management)

**Why it matters:** Every deployment, health check, and circuit breaker is a state machine.

**Core Concept:**

```
State = what you remember between events
Event = something that happened (API call, timeout, metric threshold)
Transition = state changes based on event
Action = side effect (alert, deploy, rollback)
```

**Study Materials:**

- Circuit breaker pattern (CLOSED → OPEN → HALF_OPEN)
- Deployment states (PENDING → DEPLOYING → HEALTHY → FAILED)
- Pod lifecycle (Pending → Running → Succeeded/Failed)

**Exercise 1.1: Design deployment gate state machine**
Requirements:

- Parse terraform plan output
- States: ANALYZING → WAITING_APPROVAL → APPLYING → COMPLETE
- Events: plan_parsed, approval_granted, apply_success, apply_failure
- When do you transition? What triggers rollback?

**Exercise 1.2: Pod health checker**
Requirements:

- States: STARTING → READY → DEGRADED → FAILING
- Events: health_check_success, health_check_failure
- Track consecutive failures (3 failures → FAILING state)

**Deliverable:** State diagram on paper with all transitions labeled

-----

### Day 2: Event-Driven Architecture (Decoupling)

**Why it matters:** Platform services must not block each other. Async is critical at scale.

**Core Concept:**

```
Synchronous: A calls B, A waits for B to finish (blocking)
Asynchronous: A publishes event to queue/bus, returns immediately. B processes later.

Use async when:
- B's latency shouldn't block A's response (Slack alerts)
- B might be down, A should still work (monitoring sidecar)
- Multiple consumers need same event (EventBridge → Lambda + SNS + Kinesis)
```

**Study Materials:**

- EventBridge event routing patterns
- SQS vs SNS vs EventBridge (when to use which)
- Lambda async invocation vs synchronous

**Exercise 2.1: Secret rotation propagation**
Requirements:

- Vault rotates DB password (event source)
- Need to: update K8s secrets (3 namespaces), restart pods, verify health, alert on failure
- Design event flow: What publishes? What consumes? What runs sync vs async?

**Exercise 2.2: Terraform apply audit trail**
Requirements:

- When terraform apply runs, log to: S3, CloudWatch, Slack, DataDog
- Should terraform wait for all logging to complete?
- Design event-driven flow with failure handling

**Deliverable:** Event flow diagram showing publishers, queue/bus, consumers, failure paths

-----

### Day 3: Failure Modes & Retries (Resilience)

**Why it matters:** Every external dependency (Vault, K8s API, Slack) can fail. You must handle it.

**Core Concept:**

```
Transient failure: Network blip, rate limit → Retry succeeds
Permanent failure: Service down, 404 → Retry won't help

Retry strategies:
- Exponential backoff: 1s, 2s, 4s, 8s (best for rate limits)
- Jitter: add randomness to prevent thundering herd
- Circuit breaker: stop retrying after N failures, give service time to recover
```

**Study Materials:**

- AWS SDK retry behavior (default backoff)
- Kubernetes API server rate limiting
- Vault token expiration handling

**Exercise 3.1: Init container secret fetching**
Requirements:

- Pod init container fetches secret from Vault before main container starts
- Vault might be: slow (500ms), rate-limiting (429), down (503)
- Design retry logic: How many attempts? What backoff? When do you give up?
- What’s the pod state during retries? (Init:CrashLoopBackOff)

**Exercise 3.2: ArgoCD sync with timeout**
Requirements:

- Deploy via GitOps, wait for sync completion
- ArgoCD API might be: slow, returning 503, stuck syncing
- Design timeout strategy: When do you fail the deployment? How do you alert?

**Deliverable:** Retry flow diagram with backoff calculation, timeout thresholds, failure exit paths

-----

### Day 4: Resource Pools & Quotas (Multi-tenancy)

**Why it matters:** Shared platforms need resource isolation to prevent noisy neighbors.

**Core Concept:**

```
Pool pattern:
- Pre-allocate limited resources (IP addresses, CPU, memory)
- Enforce quotas per tenant (prevent resource exhaustion)
- Track usage for chargeback/showback

Common resource pools:
- Database connection pools (max 100 connections)
- Kubernetes ResourceQuota (10 CPU per namespace)
- VPC CIDR blocks (each team gets /24 subnet)
- NAT Gateway IP pools (limited elastic IPs)
```

**Study Materials:**

- Kubernetes ResourceQuota and LimitRange
- RDS connection pool sizing
- VPC CIDR allocation strategies

**Exercise 4.1: Multi-tenant namespace quotas**
Requirements:

- Platform supports 50 teams, each gets a namespace
- Each namespace limited to: 10 CPU, 20GB RAM, 5 load balancers
- How do you enforce? What happens when team hits limit?
- Design quota request workflow (team needs more CPU)

**Exercise 4.2: Database connection pool exhaustion**
Requirements:

- RDS instance supports 100 connections
- 20 microservices share this database
- Each service creates 10 connections at startup
- What happens? (200 connections needed, only 100 available)
- Design connection pooling strategy

**Deliverable:** Quota enforcement architecture with request/approval workflow

-----

### Day 5: API Gateway Patterns (Control Plane)

**Why it matters:** Platform teams expose infrastructure via APIs, not raw Terraform/kubectl.

**Core Concept:**

```
Developer → API Gateway → Backend Service → Infrastructure (K8s, Terraform, Vault)

API Gateway provides:
- Authentication (JWT, mTLS)
- Rate limiting (prevent abuse)
- Request validation (schema enforcement)
- Audit logging (compliance)
- Circuit breaking (protect backend)
```

**Study Materials:**

- AWS API Gateway vs ALB (when to use which)
- API Gateway authorizers (Lambda, Cognito)
- Rate limiting strategies (token bucket, fixed window)

**Exercise 5.1: Namespace provisioning API**
Requirements:

- Developers hit POST /namespaces → platform creates K8s namespace + RBAC + quotas
- Should this be sync or async? (What if namespace creation takes 30s?)
- How do you authenticate? (JWT from Okta?)
- How do you rate limit? (10 requests/hour per team?)

**Exercise 5.2: API Gateway vs ALB decision**
Scenarios:

- Internal platform API (only VPC access needed)
- Public-facing API (internet access, need DDoS protection)
- WebSocket endpoint (real-time deployment logs)
  For each scenario: API Gateway or ALB? Why?

**Deliverable:** API architecture diagram with auth flow, rate limiting, backend routing

-----

### Day 6: Secrets Management Architecture

**Why it matters:** Apps need secrets. Platform provides them securely without manual kubectl.

**Core Concept:**

```
Secrets flow: Vault (source of truth) → K8s Secret → Pod

Delivery methods:
- Init container: Fetch at startup, write to file/env
- Sidecar: Continuously sync (handles rotation)
- External Secrets Operator: Controller watches Vault, updates K8s Secret

Rotation without downtime:
- Dual-password support (old + new valid for 5 min)
- Rolling restart (10% pods at a time)
- Health check before marking rotated
```

**Study Materials:**

- Vault Kubernetes auth method
- External Secrets Operator vs Vault Agent
- K8s Secret vs ConfigMap (when to use which)

**Exercise 6.1: Secret rotation propagation**
Requirements:

- DB password rotates every 30 days
- 200 microservices use this password
- Can’t restart all 200 at once (capacity limit: 20 pods/min)
- Design zero-downtime rotation

**Exercise 6.2: Secrets delivery comparison**
Compare:

- Init container + Vault Agent
- External Secrets Operator
- CSI Secret Store Driver
  For each: pros/cons, failure modes, operational complexity

**Deliverable:** Secret rotation flow diagram with gradual rollout strategy

-----

### Day 7: Week 1 Review & Synthesis

**Capstone Exercise (90 min):**
Design a **deployment gate system** that blocks risky Terraform applies.

**Requirements:**

1. Parse `terraform plan` output (count resources created/changed/destroyed)
1. Calculate risk score (destroyed = 10 points, changed = 2, created = 1)
1. If score >20, require manual approval via Slack
1. Track last 10 deployments in memory
1. If >3 failures in last hour, auto-escalate to senior engineer
1. Retry Slack webhook POST with exponential backoff

**Deliverable:**

- Architecture diagram (components, data flow)
- State machine (ANALYZING → WAITING_APPROVAL → APPLYING → COMPLETE/FAILED)
- Event flow (where is async decoupling needed?)
- Failure handling (Slack down, approval timeout, apply fails)
- Resource tracking (what state do you store? where?)

**Do not write code.** Design only. Use Week 1 patterns.

-----

## Week 2: Platform Architecture Patterns

### Day 8: Multi-Region Active-Active

**Why it matters:** Enterprise platforms need geo-redundancy and low latency globally.

**Core Concept:**

```
User → Route53 (latency-based) → Regional ALBs → Regional EKS clusters

Challenges:
- Data consistency (how to replicate state?)
- Failover detection (what triggers traffic shift?)
- Blast radius (bad deploy shouldn't hit all regions)
- Cost (running 3x infrastructure)
```

**Study Materials:**

- Route53 routing policies (latency, geoproximity, failover)
- RDS cross-region read replicas
- DynamoDB Global Tables
- EKS multi-region patterns

**Exercise 8.1: API with regional databases**
Requirements:

- API in us-east-1, eu-west-1, ap-southeast-1
- RDS primary in us-east-1, read replicas in other regions
- Writes go to primary, reads from local replica
- If primary fails, promote eu-west-1

Questions to answer:

- How does Route53 detect region health?
- What happens during primary failover? (Write downtime?)
- How do you prevent split-brain? (Two primaries accepting writes)
- Progressive rollout: How do you deploy safely across regions?

**Exercise 8.2: Multi-region Kubernetes workload**
Requirements:

- Same application running in 3 EKS clusters (one per region)
- Shared state in DynamoDB Global Table
- Deploy to us-east-1 first, then eu-west-1, then ap-southeast-1
- If error rate >1% in any region, halt rollout

Questions to answer:

- How do you orchestrate cross-region deploys? (Step Functions? Custom controller?)
- How do you measure error rate per region?
- Rollback strategy if region 2 fails after region 1 deployed?

**Deliverable:** Multi-region architecture diagram with failover flow

-----

### Day 9: Internal Developer Platform (Golden Paths)

**Why it matters:** Self-service reduces platform team toil and speeds up developers.

**Core Concept:**

```
Golden Path = Pre-configured, secure, compliant infrastructure pattern

Examples:
- "Standard web service" → ALB + ECS + RDS + monitoring + secrets
- "Batch job" → Lambda + EventBridge + DLQ
- "Data pipeline" → S3 + Glue + Athena

Developer experience:
- CLI: platform create service my-api --type=web
- API: POST /services {name: "my-api", type: "web"}
- UI: Web portal with form
```

**Study Materials:**

- Platform engineering principles (Team Topologies)
- Backstage.io (Spotify’s IDP framework)
- Terraform modules as building blocks

**Exercise 9.1: Self-service EKS namespace**
Requirements:

- Developer runs: platform create namespace my-app
- Platform provisions:
  - Namespace with ResourceQuota (10 CPU, 20GB RAM)
  - NetworkPolicy (only allow traffic from ingress)
  - PodSecurityStandard (restricted)
  - Vault policy for secrets
  - Prometheus scrape config

Questions to answer:

- How do you enforce guardrails? (OPA Gatekeeper?)
- How do you prevent quota exhaustion?
- Namespace deletion workflow (check for running pods first?)
- Multi-cluster routing (dev vs staging vs prod clusters?)

**Exercise 9.2: Golden path adoption metrics**
Requirements:

- Track which teams use golden paths vs custom infra
- Measure: time-to-first-deploy, incident rate, cost efficiency
- Goal: 80% of teams on golden paths within 6 months

Questions to answer:

- How do you collect adoption metrics?
- How do you incentivize golden path usage?
- What if a team has valid reasons to deviate?

**Deliverable:** Golden path architecture with guardrails and self-service flow

-----

### Day 10: CI/CD Pipeline Design (Enterprise Scale)

**Why it matters:** Fast, reliable pipelines are the foundation of developer productivity.

**Core Concept:**

```
Pipeline stages (fail fast principle):
1. Lint + Unit Tests (fast, no infrastructure needed)
2. Build artifacts (only if stage 1 passes)
3. Integration tests (expensive, requires test environment)
4. Deploy to staging (async, monitored)
5. Deploy to prod (gated by approval + health checks)

Key decisions:
- Where does pipeline run? (GitHub Actions, GitLab CI, Jenkins)
- How do you store artifacts? (ECR, Artifactory)
- How do you handle secrets? (Vault, AWS Secrets Manager)
- How do you gate prod deploys? (Manual approval, automated canary)
```

**Study Materials:**

- GitHub Actions vs GitLab CI vs Jenkins
- Artifact versioning strategies (semver, git SHA)
- Progressive delivery (blue/green, canary, feature flags)

**Exercise 10.1: Terraform pipeline with cost gates**
Requirements:

- Developer opens PR with Terraform changes
- Pipeline runs:
1. terraform plan
1. Cost estimation (Infracost API)
1. If cost increase >$500/month, require approval
1. On merge, terraform apply

Questions to answer:

- Where does pipeline run? (GitHub Actions? Self-hosted runner?)
- How do you store Terraform state? (S3 + DynamoDB locking)
- Approval workflow (Slack webhook? GitHub review?)
- Blast radius containment (how to prevent bad apply from affecting multiple environments?)

**Exercise 10.2: Microservices build pipeline**
Requirements:

- 200 microservices, each deploys 5-10x/day
- Build time target: <5 minutes
- Need: Docker build, push to ECR, deploy to K8s via ArgoCD

Questions to answer:

- Monorepo or polyrepo? (How does this affect build triggers?)
- Build caching strategy (Docker layer cache? Remote cache?)
- Parallel builds (can you build 10 services simultaneously?)
- Deploy orchestration (all services at once? Progressive per-service?)

**Deliverable:** CI/CD pipeline diagram with stages, gates, and artifact flow

-----

### Day 11: Observability Architecture (3 Pillars)

**Why it matters:** You can’t operate what you can’t observe. Multi-tenant platforms need per-team visibility.

**Core Concept:**

```
Three pillars:

Metrics: "What's happening?" (counters, gauges, histograms)
  Examples: error_rate, p95_latency, pod_count
  Tools: Prometheus, CloudWatch Metrics

Logs: "Why did it happen?" (debug info, stack traces)
  Examples: Error logs, audit logs, access logs
  Tools: Fluentd, CloudWatch Logs, Elasticsearch

Traces: "Where's the bottleneck?" (distributed request flow)
  Examples: Which service in a chain is slow?
  Tools: AWS X-Ray, Jaeger, OpenTelemetry
```

**Study Materials:**

- Prometheus label best practices
- CloudWatch Logs Insights queries
- Distributed tracing correlation (trace IDs)

**Exercise 11.1: Multi-tenant metrics**
Requirements:

- Platform hosts 50 teams, each with 10 microservices
- Need: Per-team dashboards, cross-team aggregates, cost attribution

Questions to answer:

- How do you partition metrics by team? (Prometheus labels: team=“team-a”)
- Log aggregation: Centralized or per-team? (CloudWatch log groups per team?)
- Tracing: How do you correlate requests across services? (X-Trace-ID header)
- Cost attribution: How do you track per-team spending? (AWS tags)

**Exercise 11.2: Alert fatigue prevention**
Requirements:

- Platform generates 1000+ alerts/day across all teams
- Many are false positives or low priority
- Need: Alert routing, de-duplication, escalation

Questions to answer:

- How do you route alerts to correct team? (Metadata in alert payload)
- De-duplication strategy (same alert firing 10x in 5 min → send once)
- Escalation policy (P1 to PagerDuty immediately, P3 to Slack with delay)

**Deliverable:** Observability architecture with metrics/logs/traces collection and routing

-----

### Day 12: Security & Compliance (Platform Guardrails)

**Why it matters:** Platform team enforces security policies so app teams don’t have to become experts.

**Core Concept:**

```
Defense in depth layers:

1. Network: VPC, security groups, NACLs
2. Identity: IAM roles, RBAC, OIDC
3. Secrets: Vault, AWS Secrets Manager, encrypted at rest
4. Compute: Pod security policies, container scanning
5. Data: Encryption at rest and in transit
6. Audit: CloudTrail, K8s audit logs, access logs
```

**Study Materials:**

- AWS Well-Architected Security Pillar
- Kubernetes Pod Security Standards
- OPA Gatekeeper policies

**Exercise 12.1: Pod Security enforcement**
Requirements:

- Block pods that: run as root, use hostPath, are privileged
- Allow: non-root user, read-only root filesystem, limited capabilities

Questions to answer:

- How do you enforce? (OPA Gatekeeper, Pod Security Standards)
- What happens when developer tries to deploy non-compliant pod? (Rejected with error message)
- Exception workflow (team needs privileged pod for valid reason?)

**Exercise 12.2: Secrets access audit**
Requirements:

- Track which services access which secrets
- Alert if: service accesses secret it shouldn’t, unusual access pattern
- Compliance requirement: 90-day retention of all access logs

Questions to answer:

- How does Vault log access? (Audit logs to CloudWatch)
- Anomaly detection (service suddenly accessing 50 secrets → alert)
- Compliance reporting (generate monthly access report per team)

**Deliverable:** Security architecture with policy enforcement points and audit flow

-----

### Day 13: Cost Optimization & FinOps

**Why it matters:** Cloud spend can spiral out of control. Platform team owns cost guardrails.

**Core Concept:**

```
FinOps principles:

1. Visibility: Tag everything, track per-team/per-service
2. Optimization: Right-size instances, use Spot, reserved capacity
3. Governance: Quotas, budgets, alerts on overspend
4. Chargeback: Attribute costs back to teams

Common cost drivers:
- Over-provisioned EC2/RDS instances
- Idle resources (dev clusters running 24/7)
- Data transfer (cross-region, cross-AZ)
- Unoptimized storage (S3 lifecycle policies)
```

**Study Materials:**

- AWS Cost Explorer
- Kubernetes resource requests vs limits
- Spot instance best practices

**Exercise 13.1: Cost attribution system**
Requirements:

- 50 teams sharing AWS account
- Need: Monthly cost report per team
- Breakdown: EC2, RDS, S3, data transfer

Questions to answer:

- How do you tag resources? (team=team-a tag on all resources)
- Shared resources (how to split cost of shared NAT Gateway?)
- Kubernetes workloads (how to attribute EKS node costs to teams based on namespace usage?)

**Exercise 13.2: Cost anomaly detection**
Requirements:

- Detect when team’s weekly spend increases >50%
- Alert team immediately with breakdown
- Suggest optimization actions

Questions to answer:

- Data source (AWS Cost and Usage Reports?)
- Anomaly detection logic (compare current week to 4-week average?)
- Alert content (which service caused spike? link to AWS console)

**Deliverable:** Cost visibility architecture with tagging strategy and alerting flow

-----

### Day 14: System Design Capstone

**Full Interview Scenario (90 min):**
Design a **CI/CD platform for 200 microservices**

**Requirements:**

- 200 services across 20 teams
- Each service deploys 5-10x/day to dev, staging, prod
- Need: Fast feedback (<5 min build), progressive rollout, auto-rollback on errors
- Constraints: AWS only, Kubernetes for compute, GitOps for deploys

**Your Task:**

### 1. High-Level Architecture (30 min)

Draw component diagram showing:

- Entry point (GitHub webhook triggers build?)
- Build orchestration (GitHub Actions? Jenkins?)
- Artifact storage (ECR for Docker images)
- GitOps tool (ArgoCD, Flux?)
- Deployment pipeline (dev → staging → prod progression)
- Observability (metrics, logs, traces)

### 2. Design Decisions (30 min)

For each decision, explain trade-offs:

**API Gateway vs ALB for platform APIs?**

- API Gateway: Built-in auth, rate limiting, request validation
- ALB: Cheaper, WebSocket support, but need custom auth
- Decision: ?

**Sync vs async namespace creation?**

- Sync: User waits for response (30s timeout risk)
- Async: Return job ID, user polls status
- Decision: ?

**EventBridge vs direct API calls between platform services?**

- EventBridge: Decoupled, durable, multi-consumer
- Direct calls: Simple, fast, but tightly coupled
- Decision: ?

**Kubernetes multi-cluster vs single large cluster?**

- Multi: Better isolation, blast radius containment
- Single: Simpler operations, resource sharing
- Decision: ?

### 3. Failure Scenarios (20 min)

How do you handle:

- GitHub is down → Can teams still deploy?
- ArgoCD stuck syncing → How to detect and alert?
- Build takes >30 min → Timeout and notification?
- RDS primary fails during deployment → Impact on pipeline?

### 4. Security & Compliance (10 min)

- Where do secrets live? (Vault)
- Who can deploy to prod? (RBAC via Okta + SAML)
- How do you audit all platform actions? (CloudTrail + K8s audit logs)
- How do you prevent privilege escalation?

**Deliverable:**

- Architecture diagram (all components, data flow, failure paths)
- 1-page design doc (key decisions, trade-offs, open questions)
- State machine for deployment lifecycle
- Security boundaries diagram

-----

## Daily Study Format (90 min)

### Phase 1: Concept Study (30 min)

- Read the day’s pattern and scenarios
- Study linked AWS/K8s documentation
- Review real-world examples (open-source projects, blog posts)

### Phase 2: Architecture Design (30 min)

- Close all references
- Draw architecture diagram on paper for the exercise
- Label all components, data flows, failure paths
- Annotate with decision rationale

### Phase 3: Verbal Explanation (30 min)

- Record yourself explaining the architecture out loud
- Pretend you’re in an interview: “Let me walk you through this design…”
- Play back the recording
- Note: unclear explanations, missing details, weak justifications
- Re-record unclear sections

-----

## Success Criteria

By end of Week 2, you should be able to:

✅ **Explain component choices** — Why API Gateway over ALB? Why EventBridge over SQS?

✅ **Trace failure paths** — What breaks when Vault is down? How do you detect and recover?

✅ **Articulate trade-offs** — Sync vs async? Centralized vs distributed? Cost vs performance?

✅ **Design with constraints** — Cost limits, latency SLAs, compliance requirements, security boundaries

✅ **Think in layers** — Network → Compute → Data → Application → Observability

✅ **Scale considerations** — What works for 10 services breaks at 200. What changes?

-----

## Interview Readiness Checklist

Before your next interview round:

### Architecture Fluency

- [ ] Can draw component diagram for any platform pattern in <10 min
- [ ] Can explain why each component is needed (no “because that’s what we use”)
- [ ] Can propose 2-3 alternative architectures and compare trade-offs

### Failure Mode Analysis

- [ ] For any component, can list 3+ failure scenarios
- [ ] Can explain detection mechanism (health checks, metrics, alerts)
- [ ] Can propose remediation (retry, failover, rollback, escalation)

### Communication Clarity

- [ ] Can explain architecture without jargon (pretend interviewer is PM, not engineer)
- [ ] Can zoom in/out (high-level flow → implementation detail → back to high-level)
- [ ] Can pause and ask: “Does this make sense before I continue?”

### Production Thinking

- [ ] Every design includes: auth, rate limiting, audit logging
- [ ] Every external call includes: timeout, retry, circuit breaker
- [ ] Every stateful component includes: backup, recovery, migration path

-----

## Red Flags to Avoid

### ❌ Don’t generate code during interviews

Platform engineering interviews are architecture discussions, not coding tests.

### ❌ Don’t memorize architectures

Memorized diagrams are obvious. You’ll fail when they ask “what if X changes?”

### ❌ Don’t skip failure scenarios

Saying “we’ll use EventBridge” without explaining what happens when EventBridge is down = incomplete design.

### ❌ Don’t over-engineer

“We’ll use 5 microservices with Kafka and…” Stop. Start simple. Add complexity only when justified.

### ❌ Don’t say “I don’t know” and stop

Say: “I don’t know the exact API, but here’s how I’d figure it out…” and reason through it.

-----

## Post-Study: Interview Simulation

After completing Week 2:

1. **Record yourself** solving Day 14 capstone (90 min, no references)
1. **Watch the recording** — Would you hire this person?
1. **Identify gaps** — What did you struggle to explain?
1. **Repeat** the weak scenarios from Week 2

**Goal:** Explain any platform pattern fluently, without notes, in <10 minutes.

-----

## Resources (Reference Only, Not During Exercises)

### AWS Documentation

- Well-Architected Framework (all 6 pillars)
- EKS Best Practices Guide
- EventBridge patterns and use cases
- API Gateway vs ALB comparison

### Kubernetes

- Resource management (requests, limits, quotas)
- Pod Security Standards
- Network Policies
- Admission Controllers (OPA Gatekeeper)

### Platform Engineering

- Team Topologies book (platform team patterns)
- Backstage.io documentation (IDP examples)
- CNCF landscape (tool categories)

### Blog Posts / Case Studies

- Spotify platform engineering blog
- Netflix engineering blog (Spinnaker, Titus)
- Datadog engineering blog (multi-tenancy at scale)

-----

## Final Note: The Meta-Skill

This study plan builds **technical communication** — the ability to transfer a mental model of a system into someone else’s head using only words and diagrams.

**This is THE skill for senior platform roles:**

- Design reviews with principal engineers
- Incident post-mortems with leadership
- Architecture RFCs for cross-team alignment
- Mentoring junior engineers

Code is just the artifact. **Reasoning is what gets you hired.**

-----

Good luck. Now close this doc and start Day 1.