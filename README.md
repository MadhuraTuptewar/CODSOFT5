# CODSOFT5 TASK-5
import tkinter as tk
from tkinter import messagebox
class Contact:
    def __init__(self, name, phone, email, address):
        self.name = name
        self.phone = phone
        self.email = email
        self.address = address

class ContactBookApp:
    def __init__(self, master):
        self.master = master
        self.master.title("Contact Book")
        self.master.geometry("500x400")
        self.master.configure(bg="#f0f0f0")

        self.contacts = []

        self.name_var = tk.StringVar()
        self.phone_var = tk.StringVar()
        self.email_var = tk.StringVar()
        self.address_var = tk.StringVar()

        self.create_widgets()

    def create_widgets(self):
        tk.Label(self.master, text="Contact Book", font=("Helvetica", 16), bg="#f0f0f0").pack(pady=10)

        # Entry Frame
        entry_frame = tk.Frame(self.master, bg="#00FFFF")
        entry_frame.pack()

        tk.Label(entry_frame, text="Name:", font=("Helvetica", 12), bg="#f0f0f0").grid(row=0, column=0, padx=10, pady=5)
        tk.Entry(entry_frame, textvariable=self.name_var, font=("Helvetica", 12)).grid(row=0, column=1, padx=10, pady=5)

        tk.Label(entry_frame, text="Phone:", font=("Helvetica", 12), bg="#f0f0f0").grid(row=1, column=0, padx=10, pady=5)
        tk.Entry(entry_frame, textvariable=self.phone_var, font=("Helvetica", 12)).grid(row=1, column=1, padx=10, pady=5)

        tk.Label(entry_frame, text="Email:", font=("Helvetica", 12), bg="#f0f0f0").grid(row=2, column=0, padx=10, pady=5)
        tk.Entry(entry_frame, textvariable=self.email_var, font=("Helvetica", 12)).grid(row=2, column=1, padx=10, pady=5)

        tk.Label(entry_frame, text="Address:", font=("Helvetica", 12), bg="#f0f0f0").grid(row=3, column=0, padx=10, pady=5)
        tk.Entry(entry_frame, textvariable=self.address_var, font=("Helvetica", 12)).grid(row=3, column=1, padx=10, pady=5)

        # Buttons Frame
        buttons_frame = tk.Frame(self.master, bg="#f0f0f0")
        buttons_frame.pack(pady=10)

        tk.Button(buttons_frame, text="Add Contact", command=self.add_contact, font=("Helvetica", 12), bg="#4caf50", fg="#ffffff").pack(side=tk.LEFT, padx=10)
        tk.Button(buttons_frame, text="View Contacts", command=self.view_contacts, font=("Helvetica", 12), bg="#2196f3", fg="#ffffff").pack(side=tk.LEFT, padx=10)
        tk.Button(buttons_frame, text="Search Contact", command=self.search_contact, font=("Helvetica", 12), bg="#ffc107", fg="#000000").pack(side=tk.LEFT, padx=10)
        tk.Button(buttons_frame, text="Update Contact", command=self.update_contact, font=("Helvetica", 12), bg="#ff9800", fg="#ffffff").pack(side=tk.LEFT, padx=10)
        tk.Button(buttons_frame, text="Delete Contact", command=self.delete_contact, font=("Helvetica", 12), bg="#e91e63", fg="#ffffff").pack(side=tk.LEFT, padx=10)

    def add_contact(self):
        name = self.name_var.get()
        phone = self.phone_var.get()
        email = self.email_var.get()
        address = self.address_var.get()

        if name and phone:
            contact = Contact(name, phone, email, address)
            self.contacts.append(contact)
            messagebox.showinfo("Success", "Contact added successfully!")
            self.clear_entries()
        else:
            messagebox.showerror("Error", "Name and Phone are required fields.")

    def view_contacts(self):
        if not self.contacts:
            messagebox.showinfo("Info", "No contacts to display.")
            return

        contacts_list = "\n".join([f"{contact.name} - {contact.phone}" for contact in self.contacts])
        messagebox.showinfo("Contacts", contacts_list)

    def search_contact(self):
        search_term = tk.simpledialog.askstring("Search", "Enter name or phone number:")
        if search_term:
            results = [contact for contact in self.contacts if search_term.lower() in contact.name.lower() or search_term in contact.phone]
            if results:
                results_list = "\n".join([f"{contact.name} - {contact.phone}" for contact in results])
                messagebox.showinfo("Search Results", results_list)
            else:
                messagebox.showinfo("Search Results", "No matching contacts found.")
        else:
            messagebox.showinfo("Info", "Search canceled.")

    def update_contact(self):
        search_term = tk.simpledialog.askstring("Update", "Enter name or phone number:")
        if search_term:
            results = [contact for contact in self.contacts if search_term.lower() in contact.name.lower() or search_term in contact.phone]
            if results:
                contact_to_update = results[0]
                self.name_var.set(contact_to_update.name)
                self.phone_var.set(contact_to_update.phone)
                self.email_var.set(contact_to_update.email)
                self.address_var.set(contact_to_update.address)
                messagebox.showinfo("Info", "Contact details populated for update.")
            else:
                messagebox.showinfo("Info", "No matching contacts found for update.")
        else:
            messagebox.showinfo("Info", "Update canceled.")

    def delete_contact(self):
        search_term = tk.simpledialog.askstring("Delete", "Enter name or phone number:")
        if search_term:
            results = [contact for contact in self.contacts if search_term.lower() in contact.name.lower() or search_term in contact.phone]
            if results:
                contact_to_delete = results[0]
                self.contacts.remove(contact_to_delete)
                messagebox.showinfo("Success", "Contact deleted successfully!")
            else:
                messagebox.showinfo("Info", "No matching contacts found for deletion.")
        else:
            messagebox.showinfo("Info", "Deletion canceled.")

    def clear_entries(self):
        self.name_var.set('')
        self.phone_var.set('')
        self.email_var.set('')
        self.address_var.set('')

def main():
    root = tk.Tk()
    app = ContactBookApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()
