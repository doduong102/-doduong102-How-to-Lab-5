# (This is Edstem I swear)

**What environment are you using (computer, operating system, web browser, terminal/editor, and so on)?**

I am running everything in a gitbash terminal within vscode on a Windows 11 machine

**Detail the symptom you're seeing. Be specific; include both what you're seeing and what you expected to see instead. Screenshots are great, copy-pasted terminal output is also great. Avoid saying “it doesn't work”.**

My code is working fine when I run I manually run it but breaks after I run it through my bash file. I'm trying to run the `Contains=` query but it keeps printing out the entire file instead of the intended behavior.

Input:
```
./runme.sh poem.txt 'Contains=haiku'
```
Expected:
```
Compiled successfully, now running...
Also a haiku
```
Actual:
```
Compiled successfully, now running...
This is a short file
It contains text and this is
Also a haiku
```

**Detail the failure-inducing input and context. That might mean any or all of the command you're running, a test case, command-line arguments, working directory, even the last few commands you ran. Do your best to provide as much context as you can.**

My program is based off of last quarter's cse11 PA7. Specifically, StringSearchMilestone3.
This is StringSearch.java's contents
```
import java.nio.file.*;
import java.io.*;
import java.util.Scanner;

class FileHelper {
    static String[] getLines(String path) {
        try {
            return Files.readAllLines(Paths.get(path)).toArray(String[]::new);
        }
        catch(IOException e) {
            System.err.println("Error reading file " + path + ": " + e);
            return new String[]{"Error reading file " + path + ": " + e};
        }
    }
}

interface Query {
    boolean matches(String s);
}

interface Transform {
    String transform(String s);
}

class ContainsQuery implements Query {
    private String query;

    public ContainsQuery(String query) {
        this.query = query;
    }

    public boolean matches(String s) {
        return s.contains(this.query);
    }

    public String toString() {
        return query;
    }
}


class StringSearch {

    static Query readQuery(String q) {

        if (q.startsWith("contains=")) {
            String query = q.substring(9).replace("'", "");
            return new ContainsQuery(query);
            
        }

        else {
            return new Query() {
                public boolean matches(String s) {
                    return true;
                }
            };
        }
    }

    public static void main(String[] args) throws FileNotFoundException {
        
        File file = new File(args[0]);
        Scanner scan = new Scanner(file);

        Query query = readQuery(args.length > 1 ? args[1] : "");

        while (scan.hasNextLine()) {
            String line = scan.nextLine();
            if (query == null || query.matches(line)) {
                System.out.println(line);
            }
        }
        
        scan.close();
    
    }
}
```
This is the poem.txt's contents:
```
This is a short file
It contains text and this is
Also a haiku
```

And this is runme.sh's contents:
```
program="StringSearch"

javac $program.java

if [ $? -eq 0 ]; then
    echo "Compiled successfully, now running..."

    java $program $1 $2
else
    echo "Failed to compile"
fi
```
*Java file pretend attached to the pretend Edstem post*

## The *Staff's* Response

Hi Student,

In your `StringSearch.java` program within the StringSearch class, your if statement checks for "contains=" and not "Contains=" like you are running in your bash terminal. Try running it with `./runme.sh poem.txt 'contains=haiku'` and see if that fixes the problem. If it's still not behaving as intended make a follow-up post.

## The Student's Response

Hi I think it worked, thank you!

Before:
<br />
![image](https://github.com/doduong102/How-to-Lab-5/assets/130004918/76f2bda3-1a65-4290-8ae9-60bb0c5a6725)

After:
<br />
![image](https://github.com/doduong102/How-to-Lab-5/assets/130004918/5679622e-c870-4de4-aca8-22dd2e3563e7)

