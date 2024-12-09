## Difference Between `TextTheme._();` and `_TextTheme();`  
*  `TextTheme._();`:
    *  The **class name is public**, but the constructor is private.
    *  This means the class can still be accessed externally (e.g., for **static** members), but instances cannot be created.
    *  
       ```
       class TextTheme {
       TextTheme._(); // Private constructor
       static const TextStyle headline = TextStyle(fontSize: 20);
       }

       // Usage:
       TextTheme.headline; // ✅ Accessible
       var theme = TextTheme(); // ❌ Error: Constructor is private
       ```
*  `_TextTheme();`:
    *  The **entire class is private**.
    *  This means the class itself is not accessible outside.
    * ```
      class _TextTheme {
      _TextTheme(); // Private class
      static const TextStyle headline = TextStyle(fontSize: 20);
      }

      // Usage:
      _TextTheme.headline; // ❌ Error: Class is private
      ```

### Conclusion: 
*  `TextTheme._();`:  Is more suitable for utility classes and follows standard Dart practices for grouping static constants or methods..
