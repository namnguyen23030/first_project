using System;
using System.Collections.Generic;

// Interface for common library item details
interface ILibraryItem 
{
    string GetDetails(); 
}

// Base class for people with Name and ID
class Person
{
    public string Name { get; set; }
    public int ID { get; set; }

    public Person(string name, int id) 
    {
        Name = name;
        ID = id;
    }
}

// Class representing a book in the library
class Book : ILibraryItem
{
    public string Title { get; set; }
    public string Author { get; set; }
    public string ISBN { get; set; }
    public bool IsCheckedOut { get; set; }

    public Book(string title, string author, string isbn)
    {
        Title = title;
        Author = author;
        ISBN = isbn;
        IsCheckedOut = false;
    }

    public void CheckOut()
    {
        IsCheckedOut = true;
    }

    public void Return()
    {
        IsCheckedOut = false;
    }

    public string GetDetails()
    {
        return $"Title: {Title}, Author: {Author}, ISBN: {ISBN}, Checked Out: {IsCheckedOut}";
    }

    public override string ToString()
    {
        return GetDetails();
    }
}

// Class representing a library member inheriting from Person
class Member : Person
{
    public List<Book> CheckedOutBooks { get; set; } 

    public Member(string name, int id) : base(name, id)
    {
        CheckedOutBooks = new List<Book>();
    }

    public void CheckOutBook(Book book)
    {
        CheckedOutBooks.Add(book);
        book.CheckOut();
    }

    public void ReturnBook(Book book)
    {
        CheckedOutBooks.Remove(book);
        book.Return();
    }
}

// Class representing the library
class Library
{
    public List<Book> Books { get; set; } 
    public List<Member> Members { get; set; }

    public Library()
    {
        Books = new List<Book>();
        Members = new List<Member>();
    }

    public void AddBook(Book book)
    {
        Books.Add(book);
    }

    public void RemoveBook(Book book)
    {
        Books.Remove(book);
    }

    public void RegisterMember(Member member)
    {
        Members.Add(member);
    }

    public void UnregisterMember(Member member)
    {
        Members.Remove(member);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Library myLibrary = new Library();

        // Add books
        myLibrary.AddBook(new Book("The Great Gatsby", "F. Scott Fitzgerald", "978-0743273565"));
        myLibrary.AddBook(new Book("To Kill a Mockingbird", "Harper Lee", "978-0061120088"));

        // Add members
        myLibrary.RegisterMember(new Member("John Doe", 1));
        myLibrary.RegisterMember(new Member("Jane Smith", 2));

        // Check out a book
        myLibrary.Members[0].CheckOutBook(myLibrary.Books[0]); 

        // Print member details
        Console.WriteLine(myLibrary.Members[0].ToString()); 
    }
}
