import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QListWidget, QLineEdit, QPushButton, QLabel, QVBoxLayout, QWidget


class ExpenseTracker(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Expense Tracker")

        # Widgets
        self.expense_list = QListWidget()
        self.expense_name_input = QLineEdit()
        self.expense_name_input.setPlaceholderText("Expense Name")
        self.expense_amount_input = QLineEdit()
        self.expense_amount_input.setPlaceholderText("Expense Amount")
        self.save_button = QPushButton("Save")
        self.total_label = QLabel("Total Amount: 0")

        # Layout
        layout = QVBoxLayout()
        layout.addWidget(self.expense_list)
        layout.addWidget(self.expense_name_input)
        layout.addWidget(self.expense_amount_input)
        layout.addWidget(self.save_button)
        layout.addWidget(self.total_label)

        # Main widget
        container = QWidget()
        container.setLayout(layout)
        self.setCentralWidget(container)

        # Logic
        self.save_button.clicked.connect(self.add_expense)
        self.total_amount = 0

    def add_expense(self):
        name = self.expense_name_input.text()
        amount_text = self.expense_amount_input.text()

        if not name or not amount_text:
            return  # Ignore if input is empty

        try:
            amount = float(amount_text)
        except ValueError:
            return  # Ignore invalid amount input

        # Add to the list
        self.expense_list.addItem(f"{name} - {amount:.2f}")

        # Update total
        self.total_amount += amount
        self.total_label.setText(f"Total Amount: {self.total_amount:.2f}")

        # Clear inputs
        self.expense_name_input.clear()
        self.expense_amount_input.clear()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = ExpenseTracker()
    window.show()
    sys.exit(app.exec_())
