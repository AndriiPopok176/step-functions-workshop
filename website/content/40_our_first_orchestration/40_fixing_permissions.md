* Define our new AWS Step Functions state machine inside a new file located at `statemachine/account-application-workflow.asl.json`

* Update our SAM template file `template.yaml` to include a resource to deploy our new state machine
➡️ Step 3. Now, we'll need to create a new file to hold our state machine definition in our filesystem. From inside `workshop-dir` run:
```bash
mkdir -p statemachine && pushd statemachine && touch account-application-workflow.asl.json && popd
```
This will create a blank `statemachine/account-application-workflow.asl.json` inside `workshop-dir`.

➡️ Step 4. Replace `statemachine/account-application-workflow.asl.json` with <span class="clipBtn clipboard" data-clipboard-target="#ida0d7df16df74104c36cb221ee8f4f61bab25ef76codevariantsstatemachine1firstversion__accountapplicationworkflowasljson">this content</span> (click the gray button to copy to clipboard). 
<div id="diff-ida0d7df16df74104c36cb221ee8f4f61bab25ef76codevariantsstatemachine1firstversion__accountapplicationworkflowasljson"></div> <script type="text/template" data-diff-for="diff-ida0d7df16df74104c36cb221ee8f4f61bab25ef76codevariantsstatemachine1firstversion__accountapplicationworkflowasljson">commit a0d7df16df74104c36cb221ee8f4f61bab25ef76
diff --git a/code/variants/statemachine/1-first-version__account-application-workflow.asl.json b/code/variants/statemachine/1-first-version__account-application-workflow.asl.json
index 0000000..ebc80ed
+++ b/code/variants/statemachine/1-first-version__account-application-workflow.asl.json
@@ -0,0 +1,31 @@
+    {
+        "StartAt": "Check Name",
+        "States": {
+            "Check Name": {
+                "Type": "Task",
+                "Parameters": {
+                    "command": "CHECK_NAME",
+                    "data": {
+                        "name.$": "$.application.name"
+                    }
+                },
+                "Resource": "${DataCheckingFunctionArn}",
+                "Next": "Check Address"
+            },
+            "Check Address": {
+                "Type": "Task",
+                "Parameters": {
+                    "command": "CHECK_ADDRESS",
+                    "data": {
+                        "address.$": "$.application.address"
+                    }
+                },
+                "Resource": "${DataCheckingFunctionArn}",
+                "Next": "Approve Application"
+            },
+            "Approve Application": {
+                "Type": "Pass",
+                "End": true
+            }
+        }
+    }
\ No newline at end of file
</script>
{{< /safehtml >}} {{< /expand >}}
{{< safehtml >}}
<textarea id="ida0d7df16df74104c36cb221ee8f4f61bab25ef76codevariantsstatemachine1firstversion__accountapplicationworkflowasljson" style="position: relative; left: -1000px; width: 1px; height: 1px;">    {
        "StartAt": "Check Name",
        "States": {
            "Check Name": {
                "Type": "Task",
                "Parameters": {
                    "command": "CHECK_NAME",
                    "data": {
                        "name.$": "$.application.name"
                    }
                },
                "Resource": "${DataCheckingFunctionArn}",
                "Next": "Check Address"
            },
            "Check Address": {
                "Type": "Task",
                "Parameters": {
                    "command": "CHECK_ADDRESS",
                    "data": {
                        "address.$": "$.application.address"
                    }
                },
                "Resource": "${DataCheckingFunctionArn}",
                "Next": "Approve Application"
            },
            "Approve Application": {
                "Type": "Pass",
                "End": true
            }
        }
    }
</textarea>
{{< /safehtml >}}

➡️ Step 5. Now, we'll update SAM's `template.yaml` file to reference our new state machine. Replace `template.yaml` with <span class="clipBtn clipboard" data-clipboard-target="#idcodevariantstemplateyml0initial__templateyamlcodevariantstemplateyml1fixingpermissions__templateyaml">this content</span> (click the gray button to copy to clipboard). 
{{< expand "Click to view diff" >}} {{< safehtml >}}
<div id="diff-idcodevariantstemplateyml0initial__templateyamlcodevariantstemplateyml1fixingpermissions__templateyaml"></div> <script type="text/template" data-diff-for="diff-idcodevariantstemplateyml0initial__templateyamlcodevariantstemplateyml1fixingpermissions__templateyaml">diff --git a/code/variants/template.yml/0-initial__template.yaml b/code/variants/template.yml/1-fixing-permissions__template.yaml
index dc57843..81aed51 100644
--- a/code/variants/template.yml/0-initial__template.yaml
@@ -3,6 +3,16 @@ Transform: AWS::Serverless-2016-10-31
 Description: Template for step-functions-workshop
 
 Resources:
   ApproveApplicationFunction:
     Type: AWS::Serverless::Function
     Properties:
<textarea id="idcodevariantstemplateyml0initial__templateyamlcodevariantstemplateyml1fixingpermissions__templateyaml" style="position: relative; left: -1000px; width: 1px; height: 1px;">AWSTemplateFormatVersion: "2010-09-09"
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
        - DynamoDBCrudPolicy:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
        - DynamoDBCrudPolicy:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable