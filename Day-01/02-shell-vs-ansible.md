# Comparison with Shell Scripting

🔹 Step 1: What is Shell Script?

Shell script = just a list of commands in .sh files 

Example:

mkdir myfolder
First time you run:

✔ Folder created

Second time you run:

❌ Error → “File exists”

Why?

Because shell does not check anything.
It simply runs the command again.

👉 It does NOT think.

🔹 Step 2: What is the Problem Here?

In DevOps, we run automation many times.

If script fails second time → deployment fails → production issue 😬

This is called Not Idempotent

🔹 Step 3: What is Idempotent?

Very simple meaning:

👉 Run many times → Same result → No error

Example idea:

You say:

“Make sure nginx is installed”

If already installed → do nothing
If not installed → install

That is idempotent behavior.

🔹 Step 4: How Ansible is Different

In Ansible, you don’t say:

“Run yum install nginx”

Instead you say:

“State: nginx should be installed”

Ansible checks first.

If installed → no change
If not installed → install

🔹 Simple Real Life Example

Imagine a light switch 💡

Shell Script behavior:

You press switch every time.
Light will ON/OFF/ON/OFF.

Unpredictable.

Ansible behavior:

You say:

“Light should be ON”

If already ON → nothing happens
If OFF → it turns ON

Always same result.

That is called:

✔ Predictable
✔ Safe
✔ Idempotent

🔹 Why Shell Script Fails When Run Twice?

Example:

useradd ganesh

First time → user created
Second time → error (user already exists)

Because shell doesn’t check.

To make it safe, you must manually write:

if id "ganesh" &>/dev/null; then
  echo "User exists"
else
  useradd ganesh
fi

Now imagine writing this for 200 tasks 😵

Very complex.

🔹 Why Ansible is Easy?

Ansible already knows how to check.

You just write:

- name: Create user
  user:
    name: ganesh
    state: present

Run 1 time → creates
Run 100 times → no error

```
#/bin/bash

set -e 

mkdir test-demo
echo "hi"
```

- Scalability and flexibility

Easily and quickly scale the systems you automate through a modular design that supports a large range of operating systems, cloud platforms, and network devices.

