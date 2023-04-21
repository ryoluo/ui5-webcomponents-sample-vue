<template>
    <div class="home">
        <div class="create-todo-wrapper">
            <ui5-input placeholder="Type a task..." ref="todoInput" class="add-todo-element-width"
                id="add-input"></ui5-input>
            <ui5-date-picker format-pattern="dd/MM/yyyy" class="add-todo-element-width" ref="todoDeadline"
                id="date-picker"></ui5-date-picker>
            <ui5-button class="add-todo-element-width" ref="addButton" design="Emphasized" @click="handleAdd">Add
                Todo</ui5-button>
        </div>

        <div class="list-todos-wrapper">
            <ui5-panel class="list-todos-panel" header-text="Incompleted Tasks" :collapsed="panelCollapsed()">
                <TodoList :todos="todos" @selection-change="handleDone" @item-deleted="handleRemove"
                    @item-edit="handleEdit">
                </TodoList>
            </ui5-panel>

            <ui5-panel class="list-todos-panel" header-text="Completed Tasks" :collapsed="donePanelCollapsed()">
                <TodoList :todos="doneTodos" @selection-change="handleUndone" @item-deleted="handleRemove"
                    @item-edit="handleEdit">
                </TodoList>
            </ui5-panel>
        </div>
        <ui5-dialog header-text="Edit Todo" ref="editDialog">
            <div class="dialog-content">
                <div class="edit-wrapper">
                    <ui5-label>Title:</ui5-label>
                    <ui5-textarea class="title-textarea" show-exceeded-text maxlength="24" :value="todoBeingEdittedText"
                        ref="titleEditInput">
                    </ui5-textarea>
                </div>

                <div class="edit-wrapper date-edit-fields">
                    <ui5-label>Date:</ui5-label>
                    <ui5-date-picker format-pattern="dd/MM/yyyy" :value="todoBeingEdittedDate"
                        ref="dateEditInput"></ui5-date-picker>
                </div>
            </div>
            <div class="dialog-footer" data-ui5-slot="footer">
                <ui5-button class="dialog-footer-btn--cancel" design="Transparent" @click="cancelEdits">Cancel</ui5-button>
                <ui5-button class="dialog-footer-btn--save" design="Emphasized" @click="saveEdits">Save</ui5-button>
            </div>
        </ui5-dialog>

    </div>
</template>

<script>
import logo from '../assets/logo.png';
import applyDirection from "@ui5/webcomponents-base/dist/locale/applyDirection.js";
import '@webcomponents/custom-elements/custom-elements.min.js'
import '@webcomponents/shadydom/shadydom.min.js'
import { setTheme } from "@ui5/webcomponents-base/dist/config/Theme";
import '@ui5/webcomponents-base/dist/features/F6Navigation';
import '@ui5/webcomponents/dist/Title';
import '@ui5/webcomponents/dist/Input';
import '@ui5/webcomponents/dist/DatePicker';
import '@ui5/webcomponents/dist/Button';
import '@ui5/webcomponents/dist/Dialog';
import '@ui5/webcomponents/dist/Panel';
import '@ui5/webcomponents/dist/Label';
import '@ui5/webcomponents/dist/TextArea';
import "@ui5/webcomponents/dist/Popover";
import '@ui5/webcomponents/dist/Switch';
import "@ui5/webcomponents-fiori/dist/ShellBar";
import "@ui5/webcomponents-fiori/dist/ShellBarItem";
import "@ui5/webcomponents/dist/Tab";
import "@ui5/webcomponents/dist/TabContainer";
import "@ui5/webcomponents-fiori/dist/Assets.js";
import '@ui5/webcomponents-icons/dist/palette.js';
import '@ui5/webcomponents-icons/dist/settings.js';
import '@ui5/webcomponents-icons/dist/sys-help.js';
import '@ui5/webcomponents-icons/dist/log.js';
import '@ui5/webcomponents-icons/dist/account.js';
import '@ui5/webcomponents-icons/dist/private.js';
import '@ui5/webcomponents-icons/dist/loan.js';
import '@ui5/webcomponents-icons/dist/globe.js';
import TodoList from '../components/TodoList.vue';

export default {
    components: {
        TodoList
    },
    data() {
        return {
            todos: [
                {
                    text: "Get some carrots",
                    id: "i1",
                    deadline: "27/7/2018",
                    done: false
                },
                {
                    text: "Do some magic",
                    id: "i2",
                    deadline: "22/7/2018",
                    done: false
                },
                {
                    text: "Go to the gym",
                    id: "i3",
                    deadline: "24/7/2018",
                    done: false
                },
                {
                    text: "Buy milk",
                    id: "i4",
                    deadline: "30/7/2018",
                    done: false
                }
            ],
            doneTodos: [
                {
                    text: "Eat some fruits",
                    id: "i5",
                    deadline: "29/7/2018",
                    done: true
                }
            ],
            logo,
            todoBeingEdittedText: "",
            todoBeingEdittedDate: "",
            selectedEditTodo: ""
        };
    },
    methods: {
        handleThemeSettingsToggle: function (event) {
            this.$refs["theme-settings-popover"].showAt(event.detail.targetRef);
        },
        handleThemeChange: function (event) {
            setTheme(event.detail.selectedItems[0].getAttribute("data-theme"));
            this.$refs["theme-settings-popover"].close();
        },
        handleProfileClick: function (event) {
            this.$refs["profile-popover"].showAt(event.detail.targetRef);
        },
        handleProfileSettingsSelect: function (event) {
            const selectedKey = event.detail.item.getAttribute('data-key');
            if (selectedKey === 'settings') {
                window['settings-dialog'].show(event.detail.targetRef);
            } else if (selectedKey === 'help') {
                window['help-dialog'].show(event.detail.targetRef);
            }
        },
        handleRtlSwitchChange: function (event) {
            document.body.dir = event.target.checked ? "rtl" : "ltr";
            applyDirection();
        },
        handleContentDensitySwitchChange: function (event) {
            if (event.target.checked) {
                document.body.classList.add('ui5-content-density-compact');
            } else {
                document.body.classList.remove('ui5-content-density-compact');
            }
        },
        handleSettingsDialogCloseButtonClick: function () {
            window['settings-dialog'].close();
        },
        handleHelpDialogCloseButtonClick: function () {
            window['help-dialog'].close();
        },
        handleAdd: function () {
            this.todos = [...this.todos, {
                text: this.$refs["todoInput"].value,
                id: (this.todos.length + 1).toString(),
                deadline: this.$refs["todoDeadline"].value,
                done: false
            }];
        },
        handleDone(event) {
            const selectedItem = event.detail.selectedItems[0];
            const selectedId = selectedItem.getAttribute("data-key");

            const newlySelected = this.todos.filter(todo => {
                return selectedId === todo.id.toString();
            })[0];

            newlySelected.done = true;
            this.doneTodos.push(newlySelected);

            this.todos = this.todos.filter(todo => {
                return selectedId !== todo.id.toString();
            });
        },
        handleUndone(event) {
            const selectedItems = event.detail.selectedItems;
            const selectedIds = selectedItems.map(item => item.getAttribute("data-key"));

            const newlyDeselected = this.doneTodos.filter(todo => {
                return selectedIds.indexOf(todo.id.toString()) === -1;
            }).map(item => {
                return { ...item, done: false };
            });

            this.doneTodos = this.doneTodos.filter(todo => {
                return selectedIds.indexOf(todo.id.toString()) > -1;
            });

            this.todos = [...this.todos, ...newlyDeselected];
        },
        handleRemove(item) {
            const filteredTodos = this.todos.filter(todo => todo.id.toString() !== item.id);
            this.todos = filteredTodos;

            const filteredDoneTodos = this.doneTodos.filter(todo => todo.id.toString() !== item.id);
            this.doneTodos = filteredDoneTodos;
        },
        handleEdit(item) {
            const matchedTodos = this.todos.filter(todo => todo.id.toString() === item.id);
            let todoObj;

            if (matchedTodos.length) {
                todoObj = matchedTodos[0];
            } else {
                todoObj = this.doneTodos.filter(todo => todo.id.toString() === item.id)[0];
            }

            this.todoBeingEdittedText = todoObj.text;
            this.todoBeingEdittedDate = todoObj.deadline;
            this.selectedEditTodo = todoObj.id;

            this.$refs["editDialog"].show();
        },
        saveEdits() {
            const edittedText = this.$refs["titleEditInput"].value;
            const edittedDate = this.$refs["dateEditInput"].value;

            this.todos = this.todos.map((todo) => {
                if (todo.id === this.selectedEditTodo) {
                    todo.text = edittedText;
                    todo.deadline = edittedDate;
                }

                return todo;
            });

            this.doneTodos = this.doneTodos.map((todo) => {
                if (todo.id === this.selectedEditTodo) {
                    todo.text = edittedText;
                    todo.deadline = edittedDate;
                }

                return todo;
            });

            this.$refs["editDialog"].close();
        },
        cancelEdits() {
            this.$refs["editDialog"].close();
        },
        panelCollapsed() {
            return !this.todos.length ? true : undefined;
        },
        donePanelCollapsed() {
            return !this.doneTodos.length ? true : undefined;
        },

    }
};

setTheme("sap_horizon");
</script>

<style>
html,
body {
    padding: 0;
    margin: 0;
    height: 100%;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
        "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
        sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.app {
    height: 100%;
    background-color: var(--sapBackgroundColor);
}

.header-toolbar {
    position: sticky;
    z-index: 42;
    background-color: rgba(255, 255, 255, 0.6);
    box-shadow: 0 4px 5px -5px #0a6ed1;
}

.app-header-logo {
    height: 2rem;
    max-height: 2rem;
}

.app-title {
    margin-inline-start: 1rem;
}

.app-bar-theming-popover {
    width: 250px;
}

.create-todo-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem 1rem;
    margin: 2rem 0;
    box-sizing: border-box;
    background-color: var(--sapObjectHeader_Background);
}

.list-todos-wrapper {
    margin: 2rem 0;
}

.list-todos-panel {
    margin: 2rem 0;
}

.li-content-actions {
    display: flex;
}

.add-todo-element-width {
    width: auto;
}

#add-input {
    flex: auto;
}

#date-picker {
    margin: 0 0.5rem 0 0.5rem;
}

.li-content {
    display: flex;
    width: 100%;
    justify-content: space-between;
    align-items: center;
}

.li-content-text {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.edit-btn {
    margin-inline-end: 1rem;
}

.dialog-content {
    max-width: 320px;
    padding: 2rem 2rem;
}

.dialog-footer {
    display: flex;
    justify-content: flex-end;
    padding: 0.25rem 0.25rem 0 0.25rem;
    border-top: 1px solid #d9d9d9;
}

.dialog-footer-btn--cancel {
    margin-inline-end: 0.25rem;
}

.title-textarea {
    height: 100px;
    display: inline-block;
    width: 100%;
}

.date-edit-fields {
    display: flex;
    flex-direction: column;
    margin-top: 1rem;
}

@media(max-width: 600px) {
    .create-todo-wrapper {
        flex-direction: column;
    }

    .add-todo-element-width {
        width: 100%;
    }

    #date-picker {
        margin: 0.5rem 0 0.5rem 0;
    }
}

.app-bar-profile-popover {
    width: 250px;
}

#settings-dialog {
    max-width: 300px;
}

.dialog-button {
    display: flex;
    justify-content: flex-end;
    margin-top: 0.625rem;
    margin-bottom: -0.425rem;
}

.profile-settings,
.help-header {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
}

.profile-text {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-inline-start: 1rem;
}

.app-header-logo {
    height: 2rem;
}

.profile-settings-list {
    margin-top: 1.25rem;
}

.aligned {
    align-items: center;
    gap: 0.725rem;
}

.help-dialog-text {
    font-size: 0.875rem;
}

.profile-rtl-switch {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

#header-title-align {
    margin: 1rem 0;
    gap: 0.225rem;
}

#header-logo-align {
    margin: 0.225rem 3.225rem 0.225rem 0;
    align-items: center;
    gap: 0.435rem;
}

#help-dialog::part(header) {
    justify-content: flex-start;
}
</style>
