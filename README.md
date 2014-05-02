Artist.java
===========
package artpricingsystem;
import java.io.*;


 //@author jessicaspalding

class Artist {

private String artistFirstName;
private String artistLastName ;
private int fashionabilityValue;
private Artist[] arr;

    //Desc: constructor for Artist
    //Post: allows class to set the value of the artistFirstName, artistLastName,
    //		and fashionabilityValue field in a record and add it to the array 
    //      of Artist objects
    Artist(String f, String l, int v)
    {
        artistFirstName=f;
        artistLastName=l;
        fashionabilityValue=v;
        //read("Artist.txt")
	//arr.add(this)
    }

    //Desc: constructor for Artist
    //Post: allows class to set a new record and add it to the array of
    //      Artist objects
    Artist()
    {
        //read("Artist.txt")
        //arr.add(this)       
    }

    //Desc: allows class to access the artistFirstName field in a record
    //Return: artistFirstName
    public String getArtistFirstName()
    {
        return artistFirstName;
    }
	
    //Desc:allows class to set  value of the artistFirstName field in a record
    //Post: artistFirstName is set to f
    public void setArtistFirstName(String f)
    {
        artistFirstName=f;
    }

    //Desc: allows class to access the artistLastName field in a record
    //Return: artistLastName	
    public String getArtistLastName()
    {
        return artistLastName;
    }

    //Desc:allows class to set the value of the artistLastName field in a record
    //Post: dateOfPurchase is set to l
    public void setArtistLastName(String l)
    {
        artistLastName=l;
    } 

    //Desc: allows class to access the fashionabilityValue field in a record
    //Return: fashionabilityValue	
    public int getArtistFashionabilityValue()
    {
        return fashionabilityValue;
    }

    //Desc:allows class to set  value of fashionabilityValue field in a record
    //Post: dateOfPurchase is set to v
    public void setArtistFashionabilityValue(int v) 
    {
        fashionabilityValue=v;
    }

    //Desc: Finds the record the user wants to update and updates the 
    //artistFirstName field in the found object
    //Post: artistsFirstName field is updated 
    public void updateArtistsFirstName(String lname, String fname, String first)
    {
        Artist foundObj=findObject(lname, fname); //don't think the find works yet
        foundObj.setArtistFirstName(first);
        foundObj.save();
    }

    //Desc: Finds the record the user wants to update and updates the 
    //artistLastName field in the found object
    //Post: artistLastName field is updated
    public void updateArtistLastName(String lname, String fname, String last)
    {
        Artist foundObj=findObject(lname, fname);
        foundObj.setArtistFirstName(last);
        foundObj.save();
    }	

    //Desc: Finds the record the user wants to update and updates the 
    //fashionabilityValue field in the found object
    //Post: fashionabilityValue field is updated
    public void updateFashionabilityValue(String ln, String fn, String fash)
    {
            Artist foundObj=findObject(ln, fn); //if changing the find, change this
            foundObj.setArtistFirstName(fash); 
            foundObj.save();
    }	

//Desc:uses the last name and the first name of an artist to find the object
// in the array 
//Return: returns the found Artist object or null value if Artist not found
    public Artist findObject(String alastname, String afirstname) //I don't think this works
    {
  // find locates a given investment record if it exists.
  // Returns true if the investment is located, otherwise false.
        try
        {
            File artistFile = new File ("Artist.dat");
            boolean	found = false;		// result of comparison

            if (artistFile.exists ())
            {
              RandomAccessFile inFile = new RandomAccessFile (artistFile, "r");

              while (!found && (inFile.getFilePointer () != inFile.length ()))
              {
                  read (inFile);

                  //if (.compareTo (findInvestmentID) == 0) found = true; make work for ours
              }
              inFile.close();
            }

            return found; //returns boolean right now, not artist
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Investment.find () *****");
            System.out.println ("\t" + e);

            return false; //returns boolean right now, not artist
        }

  }
            /* old way for(int i=0; arr.length<i; ++i )
            {
                if(arr[i].getArtistLastName().equals(alastname) && 
                arr[i].getArtistFirstName().equals(afirstname)) //if the key matches
                    return arr[i]; //return the object
            }
            return null;
    */

//Desc: uses the last name and the first name of an artist to find the index
// of the desired object in the array 
//Return: returns the found Artist object's index in the array or null value 
// if Artist not found
public int findIndex(String alastname, String afirstname)
    {
        for(int i=0; arr.length<i; ++i )
        {
            if(arr[i].getArtistLastName().equals(alastname) && 
            arr[i].getArtistFirstName().equals(afirstname)) //if the key matches
                return i; //return the index
        }
        return -1;
    }

    //Desc: reads in a file to the scanner, sets each field in the text
    //		    file to a field in a new Artist object
    //Pre: file must exist 
//Post: an array full of Artist objects is created
    public void read(RandomAccessFile fileName)// throw exception FileNotFoundException //may not use this
    {
  // reads an investment record from fileName.
  // Assumes that the existence of fileName has already been established.
  //
        try
        {
            String  inputString = new String ();	// for storing artist record
            int	i = 0;		                // position in record

            inputString = fileName.readLine ();

            StringBuffer input = new StringBuffer ();	// for storing field within record

            while (inputString.charAt (i) != ' ')
            {
              input.append (inputString.charAt (i));
              i++;
            }

            artistFirstName = input.toString ();
            i++;

            input = new StringBuffer ();
            while (inputString.charAt (i) != ' ')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistLastName = input.toString ();
            i++;

            input = new StringBuffer ();
            while (inputString.charAt (i) != ' ')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            Integer tempInt = new Integer (input.toString ());
            fashionabilityValue = tempInt;
            i++;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Arist.read () *****");
            System.out.println ("\t" + e);
        }

      }
    
        /* old way Scanner input=new Scanner(RandomAccessFile);
        //allows the file to be read in
        int count=0;
        while(input.hasNextLine()) 
        //loops through each line in the file
        {	
            Artist newArtist=new Artist(input.next(), input.next(), input.nextInt()); 
            //creates a new Artist object
        }	
        //close input;
    }
*/
    //Desc: writes the variables artistFirstName, artistLastName,
    //		and fashionabilityValue to a record line in the specified file
    //Post: updates the specified file
    public void write(RandomAccessFile fileName)
    {
        try
        {
            fileName.writeChars (artistFirstName + " " + artistLastName + " ");
            fileName.writeInt(fashionabilityValue);
            fileName.writeChars(" " + "\n");
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Artist.write () *****");
            System.out.println ("\t" + e);
        }
    }
        /* old way boolean deleted=RandomAccessFile.delete();
        //clears file so that a new file can be written to this location
            if(deleted)  
            {
                for(int i=0; arr.length<i; ++i )
                {
                        if(arr[i].) //if an Artist record resides in this index
                        {
                            //print each record to RandomAccessFile
                            String fname=arr[i].getArtistFirstName();
                            string lname=arr[i].getArtistLastName();
                            int fash=arr[i].getFashionabilityValue();
                            output.format("%-12s%-12s%-6d%n/", fname, lname, fash); 
                            //writes 3 fields with formatting to fit allocated chars
                        }
                        else close RandomAccessFile
                }
            }
        }*/

//Desc: informs the user that the file they updated has 
//      been saved with an output message
//Pre: the file must exist
//Post: the message informing the user has been printed
    public void save()
    {
  // saves an individual artist record into a file, ordered by artistLastName.
  //
        try
        {
            File artistFile = new File ("artist.dat");	// file of artist
            File  tempArtistFile = new File ("artist.tmp");	// temporary file for artist

            Artist overwriteArtist = new Artist ();	// record read, then written
            boolean	found = false;		// terminates while-loop

            RandomAccessFile newFile = new RandomAccessFile (tempArtistFile, "rw");

            if (!artistFile.exists ())
            {
              write(newFile);
            }
            else
            {
                RandomAccessFile oldFile = new RandomAccessFile (artistFile, "r");

                int compareArtist; // to find correct place for the new investment

                while (oldFile.getFilePointer () != oldFile.length ()) //the pointer hasn't reached the end of the file
                {
                    overwriteArtist.read(oldFile); //read walks through each field in the record

                    if (artistFirstName.equals(overwriteArtist.getArtistFirstName()) &&
                    artistLastName.equals(overwriteArtist.getArtistLastName()))
                        compareArtist=1;
                    else compareArtist=0;

                    if (compareArtist==0) // the artist wasn't found in this record
                    {
                        write (newFile); //writes to temp file 
                        overwriteArtist.write (newFile); //writes to a 2nd temp file in case never found
                    }
                    else if (compareArtist ==1) 
                    {
                        write (newFile); //writes to temp file
                    } 
                    else
                    {
                      overwriteArtist.write (newFile); //writes to 2nd temp file
                    }
              }  // while

              //if (compareArist==0) write (newFile); // if never found in file, write to temp file

              oldFile.close ();

            }  // else

            newFile.close ();

            artistFile.delete ();
            tempArtistFile.renameTo (artistFile);

          }
          catch (Exception e)
          {
              System.out.println ("***** Error: Artist.putRecord () *****");
              System.out.println ("\t" + e);
          }

      }
        /* File artist=new File(A)
        write(); //need to create the file here
        System.out.println("Your record has been saved to the file");
    }    
}*?
