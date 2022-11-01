---
permalink: posts/{{ title | slug }}/index.html
title: Single Responsibility Pattern
date: 2022-10-31T22:00:00Z
tags:
- SOLID
description: Every class, module, or function in a program should have one responsibility/purpose
  in a program. As a commonly used definition, "every class should have only one reason
  to change

---
Every class, module, or function in a program should have one responsibility/purpose in a program. As a commonly used definition, "every class should have only one reason to change

### Frequency and Effects of Changes

We all know that requirements change over time. Each of them also changes the responsibility of at least one class. The more responsibilities your class has, the more often you need to change it. If your class implements multiple responsibilities, they are no longer independent of each other.

You need to change your class as soon as one of its responsibilities changes. That is obviously more often than you would need to change it if it had only one responsibility.

That might not seem like a big deal, but it also affects all classes or components that depend on the changed class. Depending on your change, you might need to update the dependencies or recompile the dependent classes even though they are not directly affected by your change. They only use one of the other responsibilities implemented by your class, but you need to update them anyway.

In the end, you need to change your class more often, and each change is more complicated, has more side-effects, and requires a lot more work than it should have. So, it’s better to avoid these problems by making sure that each class has only one responsibility.

    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.IO;
    using static System.Console;
    
    namespace DotNetDesignPatternDemos.SOLID.SRP
    {
      // just stores a couple of journal entries and ways of
      // working with them
      public class Journal
      {
        private readonly List<string> entries = new List<string>();
    
        private static int count = 0;
    
        public int AddEntry(string text)
        {
          entries.Add($"{++count}: {text}");
          return count; // memento pattern!
        }
    
        public void RemoveEntry(int index)
        {
          entries.RemoveAt(index);
        }
    
        public override string ToString()
        {
          return string.Join(Environment.NewLine, entries);
        }
    
        // breaks single responsibility principle
        public void Save(string filename, bool overwrite = false)
        {
          File.WriteAllText(filename, ToString());
        }
    
        public void Load(string filename)
        {
          
        }
    
        public void Load(Uri uri)
        {
          
        }
      }
    
      // handles the responsibility of persisting objects
      public class Persistence
      {
        public void SaveToFile(Journal journal, string filename, bool overwrite = false)
        {
          if (overwrite || !File.Exists(filename))
            File.WriteAllText(filename, journal.ToString());
        }
      }
    
      public class Demo
      {
        static void Main(string[] args)
        {
          var j = new Journal();
          j.AddEntry("I cried today.");
          j.AddEntry("I ate a bug.");
          WriteLine(j);
    
          var p = new Persistence();
          var filename = @"c:\temp\journal.txt";
          p.SaveToFile(j, filename);
          Process.Start(filename);
        }
      }
    }

asd