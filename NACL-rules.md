For Network ACLs (NACLs) in AWS, the rule with the lowest number takes precedence.

### Key Points about NACLs Rule Evaluation:

1. **Rule Order:**
   - NACL rules are evaluated in ascending order, starting from the lowest rule number to the highest.

2. **First Match Wins:**
   - As soon as a matching rule is found, the action specified by that rule (allow or deny) is applied to the traffic, and no further rules are evaluated.

3. **Rule Numbering:**
   - You can number your rules from 1 to 32766. AWS recommends leaving gaps between rule numbers (e.g., 100, 200, 300) so you can insert new rules later without needing to renumber existing rules.

4. **Default Rule:**
   - Each NACL has a default rule that is implicit and has the highest rule number (32767). This default rule denies all traffic that isn't explicitly allowed by the preceding rules.

### Example:

Assume you have the following rules in your NACL:

| Rule Number | Type      | Protocol | Port Range | Source       | Allow/Deny |
|-------------|-----------|----------|------------|--------------|------------|
| 100         | HTTP      | TCP      | 80         | 0.0.0.0/0    | ALLOW      |
| 200         | SSH       | TCP      | 22         | 203.0.113.0/24| ALLOW      |
| 300         | ALL Traffic| ALL     | ALL        | 0.0.0.0/0    | DENY       |

### How Rules Are Applied:

1. **HTTP Request on Port 80:**
   - The NACL will evaluate the rules in ascending order.
   - It will match rule 100 and allow the traffic.

2. **SSH Request on Port 22:**
   - The NACL will match rule 200 and allow the traffic.

3. **Other Traffic (e.g., RDP on Port 3389):**
   - The NACL will evaluate rules 100 and 200, find no match, and finally apply rule 300, which denies the traffic.

### Default Rule `*`

- **Implicit Deny:** The rule with the number `*` is not explicitly visible in the NACL configuration, but it exists by default.
- **Highest Precedence:** It is implicitly considered to have the highest rule number (32767), meaning it is evaluated last.
- **Function:** This rule denies any traffic that hasn't been allowed by a lower-numbered rule.

### Example of NACL Rules

Let's say you have the following NACL rules:

| Rule Number | Type      | Protocol | Port Range | Source         | Allow/Deny |
|-------------|-----------|----------|------------|----------------|------------|
| 100         | HTTP      | TCP      | 80         | 0.0.0.0/0      | ALLOW      |
| 200         | SSH       | TCP      | 22         | 203.0.113.0/24 | ALLOW      |
| *           | ALL Traffic| ALL     | ALL        | 0.0.0.0/0      | DENY       |

### How Rules Are Evaluated

1. **HTTP Request on Port 80:**
   - The NACL evaluates rule 100, which allows the traffic.
   
2. **SSH Request on Port 22:**
   - The NACL evaluates rule 200, which allows the traffic.

3. **Other Traffic (e.g., RDP on Port 3389):**
   - The NACL evaluates rules 100 and 200, finds no match, and then falls through to the implicit rule `*`, which denies the traffic.

### Summary:
- **Lowest Number Wins:** NACL rules are evaluated from the lowest number to the highest.
- **First Match Applies:** The first rule that matches the traffic determines the action.
- **Default Deny Rule:** Any traffic not explicitly allowed by a lower-numbered rule is implicitly denied by the default rule (number 32767).
- **Rule `*`:** This implicit rule denies all traffic not explicitly allowed by other rules.
- **Evaluation Order:** The NACL evaluates rules in ascending order of their rule numbers.
- **Default Deny:** Any traffic that does not match an explicit allow rule is denied by the default rule `*`.

Understanding this implicit deny rule helps ensure that your NACL configurations are secure by default, only allowing traffic that you explicitly permit.
