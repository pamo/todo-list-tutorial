# Remove item

The user should be able to remove any item, whether it's still active or completed. Revoving an item will be done by clicking a button, aptly named "remove". In this tutorial, we'll learn how to add this functionality to our project.

## File: item.component.ts

First, we need to add the button to the item, so we'll work on the file _item.component.ts_.

\(a\) Add a **\(click\)** event to the **remove** button in the item template:

```text
<button (click)="removeItem()">
  remove
</button>
```

\(b\) Add a new output to the ItemComponent class, which will be emitted to the list manager when a user pressed the remove button for a specific item:

```text
@Output() remove:EventEmitter<any> = new EventEmitter();
```

Make sure that we import both EventEmitter and Output in our class:

```text
import { Component, OnInit, Input, EventEmitter, Output } from '@angular/core';
```

\(c\) Add a function to the ItemComponent class that will actually emit the event. This function will be called when the user clicks the **remove** button:

```text
removeItem() {
  this.remove.emit(this.todoItem);
}
```

## File: list-manager.component.ts

Now that each todo item can emit its own removal, let's make sure that the list manager actually removes that same item from the list. For that, we'll work on the file _list-manager.component.ts_.

\(a\) We need to respond to **remove** event - let's add it to the template, inside the `<todo-item>` tag:

```text
(remove)="removeItem($event)"
```

\(b\) Now we just need to add the function _removeItem\(\)_ to the ListManagerComponent class:

```text
removeItem(item) {
  this.todoList = this.todoListService.removeItem(item);
}
```

## File: todo-list.service.ts

Removing the item is handled in the service - open _todo-list.service.ts_ and add a function called removeItem\(\) to the TodoListService class:

```text
removeItem(item) {
  return this.storage.destroy(item);
}
```

This function calls the destroy\(\) method we already created in todo-list-storage.service.ts earlier.

