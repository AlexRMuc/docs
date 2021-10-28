---
id: action
title: Action
slug: /field-types/action
---

Action columns allow you to run scripts in nodejs environment. There two modes
for action columns.

  - Action Script
  - Callable

### Action Script Mode

Action scripts are executed on [RowyRun](../rowy-run) and don't require any
build process, However they can not use external npm packages.

#### Usage example
Setting a user roles in firebase auth custom claims
```javascript
const { roles } = row;
const user = await auth.getUser(ref.id);
const customClaims = {
  ...user.customClaims,
  roles,
};
await auth.setCustomUserClaims(ref.id, customClaims);

return {
  success: true,
  status: `updated roles:${roles.join(", ")}`,
  message: `${row.firstName} has ${roles.join(", ")} roles now`,
  cellValue: {
    redo: true,
    status: `updated roles:${roles.join(", ")}`,
    completedAt: serverTimestamp(),
    meta: { ranBy: context.auth.token.email },
    undo: false,
  },
};
```

### Callable Mode

Callable mode requires you to build cloud functions that are compatible with Rowy action columns, using the [Rowy Actions](https://www.npmjs.com/package/rowy-actions) npm package, It's used as an
alternative to directly using `functions.https.onCall function`, it can be
installed and used in an existing firebase cloud functions project, and be used for more complex functionality that can not be achieved using actionScripts.

#### Usage

Example structure of how callable using rowy-actions package, the example illustrates how to disable a Firebase Auth user account with a callable.

```javascript
import * as admin from "firebase-admin";
const auth = admin.auth();

import callableAction from "rowy-actions";
export const SuspendUser = callableAction(async ({row, callableData}) =>{
  const {action} = callableData;
  const {firstName, email} = row;
  // switch statement can be used to perform different event based on the state of the action cell
  const user = await auth.getUserByEmail(email);
  switch (action) {
    case "run":
    case "redo":
      // both run and redo preform the same action; disabling the user's account from firebase auth
      await auth.updateUser(user.uid, {disabled: true});
      return {success: true, // return if the operation was success
        message: `${firstName}'s account has been disabled`, // message shown in snackbar on the rowy ui after the completion of action
        cellStatus: "disabled", // optional cell label, to indicate the latest state of the cell/row
        newState: "undo", // "redo" | "undo" | "disabled" are options set the behavior of action button next time it runs
      };
    case "undo":
      // re-enable user's firebase account
      await auth.updateUser(user.uid, {disabled: false});
      return {success: true, // return if the operation was success
        message: `${firstName}'s account has been re-enabled`, // message shown in snackbar on the rowy ui after the completion of action
        cellStatus: "active", // optional cell label, to indicate the latest state of the cell/row
        newState: "redo", // "redo" | "undo" | "disabled" are options set the behavior of action button next time it runs
      };
    default:
      // return error message when no action is preformed
      return {success: false, message: "Reached undefined state", newState: "redo"};
  }
});
```

