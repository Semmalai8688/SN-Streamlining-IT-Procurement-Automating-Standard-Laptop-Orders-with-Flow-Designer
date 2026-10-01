# Implement Client Script & UI Policy (Incident)

## Problem Statement

Incident records require consistent and accurate data entry for effective triage, routing, and resolution. Manual checks can result in incomplete, inconsistent, or incorrect data. This project uses ServiceNow UI Policies and Client Scripts to enforce conditional field behavior and validation directly at the user interface level.

## Objective

The objective of this project is to demonstrate how ServiceNow client-side controls can enforce data integrity on Incident records.

The implementation demonstrates how to:

- Dynamically make fields mandatory
- Auto-populate field values
- Control field behavior
- Prevent record submission when required conditions are not met
- Validate Incident records before submission

## Skills

- Incident Management
- UI Policy
- UI Policy Actions
- Client Scripts
- Form Validation

## Implementation

### Task 1: Create UI Policy on Incident

**UI Policy Name:** `High Impact Control`

**Configuration:**

- Table: `Incident`
- Active: `true`
- Field: `Impact`
- Operator: `is`
- Value: `1 – High`
- UI Policy Action: `Assignment group`
- Mandatory: Checked
- Reverse if false: `true`

This policy is triggered when the Incident Impact is set to High.

### Task 2: Create UI Policy Action – Urgency

Create a UI Policy Action under the **High Impact Control** policy.

**Configuration:**

- Field name: `Urgency`
- Read-only: `true`
- Visible: Leave unchanged

When Impact is High, the Urgency field becomes read-only.

### Task 3: Create onChange Client Script

**Name:** `Auto set urgency for high impact`

**Configuration:**

- Table: `Incident`
- Type: `onChange`
- Field name: `Impact`
- Active: `true`

**Script:**

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage('Urgency set to High for High impact incident.');
    }
}
```

When Impact is changed to High, the script automatically sets Urgency to High.

### Task 4: Create onSubmit Client Script

**Name:** `Prevent save if Assigned To missing`

**Configuration:**

- Table: `Incident`
- Type: `onSubmit`
- Active: `true`

**Script:**

```javascript
function onSubmit() {
    if (g_form.getValue('impact') == '1' &&
        g_form.getValue('assigned_to') == '') {
        g_form.showErrorBox(
            'assigned_to',
            'Assigned To is mandatory for High impact incidents.'
        );
        return false;
    }

    return true;
}
```

This script prevents an Incident from being saved when Impact is High and Assigned To is empty.

### Task 5: Create onCellEdit Client Script

**Name:** `Prevent state change via list edit`

**Configuration:**

- Table: `Incident`
- Type: `onCellEdit`
- Field name: `State`
- Active: `true`

**Script:**

```javascript
function onCellEdit(sysIDs, table, oldValues, newValue, callback) {
    alert('State cannot be updated using list editing. Please open the Incident.');
    callback(false);
}
```

This prevents users from changing the State field directly from the Incident list.

## Testing

### Test 1: Mandatory Enforcement

1. Open **Incident → Create New**.
2. Set Impact to **High**.
3. Leave Assigned To empty.
4. Click Submit.
5. Verify that the Incident is not saved and an error message is displayed.

### Test 2: Successful Save

1. Fill in the Assigned To field.
2. Click Submit.
3. Verify that the Incident is saved successfully.
4. Confirm that urgency auto-setting, read-only behavior, and State restrictions work correctly.

### Test 3: Reverse Condition

1. Open an Incident where Impact is High.
2. Change Impact from High to Medium.
3. Verify that Assigned To is no longer mandatory.
4. Verify that Urgency becomes editable.
5. Save the record.

### Test 4: List Edit Blocking

1. Go to **Incident → All**.
2. Double-click the State field.
3. Verify that an alert appears.
4. Confirm that the State value remains unchanged.

### Test 5: Form-Based Update

1. Open an Incident record.
2. Change the State field from the Incident form.
3. Click Update.
4. Verify that the State change is saved successfully.

## Expected Results

- Assignment-related mandatory behavior is enforced for high-impact incidents.
- Urgency is automatically set to High when Impact is High.
- Urgency becomes read-only when the UI Policy condition is met.
- Incidents cannot be saved without Assigned To when Impact is High.
- Direct State changes through list editing are blocked.
- State changes made through the Incident form are allowed.
- UI Policy behavior is reversed when the Impact condition is no longer true.

## Conclusion

The **Implement Client Script & UI Policy (Incident)** project demonstrates how UI Policies and Client Scripts can work together to enforce dynamic field behavior, automate updates, and prevent incorrect data submission on Incident forms. The use of `onChange`, `onSubmit`, and `onCellEdit` Client Scripts together with UI Policy actions helps maintain consistent and valid Incident data while improving form usability.

## Project Reference

This README is based on the provided project document, **Implement Client Script & UI Policy (Incident)**. fileciteturn0file0L2-L18
