package artpricingsystem;
import java.io.*;
import java.util.Date;
import java.util.Scanner;

class Artist {

private String artistFirstName;
private String artistLastName ;
private int fashionabilityValue;

    //Desc: constructor for Artist
    //Post: allows class to set the value of the artistFirstName, artistLastName,
    //		and fashionabilityValue field into this object
    Artist(String f, String l, int v)
    {
        artistFirstName=f;
        artistLastName=l;
        fashionabilityValue=v;
    }

    //Desc: no arg constructor for Artist
    //Post: instentiates a blank Artist object
    Artist() 
    {
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

    //Desc: updates an artist's first name in a record from user's input
    //Post: artistsFirstName field is updated 
    public void updateArtistsFirstName()
    {
        System.out.println("Old Artist First Name:" + artistFirstName);
        System.out.println("Please enter new Artist First Name and press <ENTER>: ");
        artistFirstName=UserInterface.getString();
    }

    //Desc: updates an artist's last name in a record from user's input
    //Post: artistLastName field is updated
    public void updateArtistLastName()
    {
        System.out.println("Old Artist Last Name:" + artistLastName);
        System.out.println("Please enter new Artist Last Name and press <ENTER>: ");
        artistLastName=UserInterface.getString();
    }	

    //Desc: updates an artist's fashionability value in a record from user's input
    //Post: fashionabilityValue field is updated
    public void updateFashionabilityValue()
    {
        try 
        {
            System.out.println("Old Fashionability Value:" + fashionabilityValue);
            System.out.println("Please enter new Fashionability Value and press <ENTER>: ");
            if (fashionabilityValue>=0 && fashionabilityValue>10000)
            {
                System.out.println("Value out of range.  Please select integer value between 0 and 10,000: ");
                fashionabilityValue=UserInterface.getInt();
            }
        
            fashionabilityValue=UserInterface.getInt();
        }
        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not an integer Value. Please select"
                   + " an integer value between 0 and 10,000: "); 
           fashionabilityValue=Integer.parseInt(UserInterface.getString());
           return;
        }
    }	

    //Desc:uses the last and first name of an artist to find a record in the file
    //Return: returns true if record is found or false if record is not found
    public boolean find(String afirstname, String alastname)
    {
        try
        {
            File artistFile = new File ("artist.dat");
            boolean found = false;
            if (artistFile.exists())
            {
                RandomAccessFile inFile = new RandomAccessFile (artistFile, "r");
                
                
                while (!found && (inFile.getFilePointer()!=inFile.length()))
                {
                    read (inFile);
                    if (artistLastName.equalsIgnoreCase(alastname) && 
                    artistFirstName.equalsIgnoreCase(afirstname))
                        found = true;       
                }
                inFile.close();
            }
            return found;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: Artist.find () *****");
            System.out.println ("\t" + e);
            return false;
        }
    }
    
    //Desc: reads a single artist record into an object from the fileName
    //Pre: RandomAccessFile must exist 
    //Post: artistLastName, artistFirstName and fashionabilityValue are read into this object
    public void read(RandomAccessFile fileName)
    {
        try
        {
            String  inputString = new String ();	
            int	i = 0;		                
            inputString = fileName.readLine ();
            StringBuffer input = new StringBuffer ();	
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistFirstName = input.toString();
            i++;
            
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistLastName = input.toString ();
            i++;
            
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
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
            return;
        }
    }

    //Desc: writes the variables artistFirstName, artistLastName,
    //	and fashionabilityValue to a record line in the specified file
    //Post: updates the specified file
    public void write(RandomAccessFile fileName)
    {
        try
        {
            fileName.writeBytes(artistFirstName + "|");
            fileName.writeBytes(artistLastName + "|");
            String fashValue="";
            fashValue=fashValue.valueOf(fashionabilityValue);
            fileName.writeBytes(fashValue+"|" + "\n");
        }
        catch (IOException e)
        {
            System.out.println ("***** Error: Artist.write () *****");
            System.out.println ("\t" + e);
        }
    }
 
    //Desc: saves an individual artist record into a file.  Will save over an
    //      existing record if the last name and first name match or will add
    //      a new record if no existing record matches this object.
    //Pre:  all fields must be valid
    //Post: the message informing the user has been printed and the artist.dat
    //      file will be modified.
    public void save()
    {
        try
        {
            File artistFile = new File ("artist.dat");	
            File  tempArtistFile = new File ("artist.tmp");	
            Artist tempArtist = new Artist ();	
            boolean found = false;		
            RandomAccessFile newFile = new RandomAccessFile (tempArtistFile, "rw");

            if (!artistFile.exists ())
            {
              write(newFile);
            }
            else
            {
                RandomAccessFile oldFile = new RandomAccessFile (artistFile, "r");
                boolean compareArtist;
                while (oldFile.getFilePointer () != oldFile.length ()) 
                {
                    tempArtist.read(oldFile);
                    if (artistFirstName.equalsIgnoreCase(tempArtist.getArtistFirstName()) &&
                    artistLastName.equalsIgnoreCase(tempArtist.getArtistLastName()))
                        compareArtist=true;
                    else compareArtist=false;
                    if(compareArtist) 
                    {
                        write (newFile); 
                        found=true;
                    } 
                    else
                    {
                      tempArtist.write(newFile); 
                    }
                }  
                if (!found) write (newFile); 

              oldFile.close ();

            }

            newFile.close ();

            artistFile.delete ();
            tempArtistFile.renameTo (artistFile);
            System.out.println("record saved to file");

          }
          catch (Exception e)
          {
              System.out.println ("***** Error: Artist.putRecord () *****");
              System.out.println ("\t" + e);
          }
    }
    
    //Desc: reads in all fields into an Artist object from user input
    //Post: artistFirstName, artistLastName, and fashionabilityValue are modified
    public void readInRecord()
    {
        try
        {     		
            System.out.println("Enter Artist First name: ");
            artistFirstName = UserInterface.getString();
            System.out.println("Enter Artist Last name: ");
            artistLastName= UserInterface.getString();
            System.out.println("Enter Fashionability Value: ");
            fashionabilityValue=Integer.parseInt(UserInterface.getString());
            while (fashionabilityValue<0 && fashionabilityValue>10000)
            {
                System.out.println("Value out of range.  Please select integer value between 0 and 10,000: ");
                fashionabilityValue=UserInterface.getInt();
            }
        }
        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not an integer Value. Please select"
                   + " an integer value between 0 and 10,000: ");
           fashionabilityValue=Integer.parseInt(UserInterface.getString());
           return;
        }
        /*catch(Exception e)
        {
            System.out.println ("***** Error: Investment.readInvestmentData () *****");
            System.out.println ("\t" + e);
        }*/
    }
    
    //Desc: Deletes a single record in the artist.dat File if the artist last 
    //      name and first name matches this object.
    //Post: Modifies the artist.dat File
    public void performDeletion () 
    {
        try
        {
            File  artistFile = new File ("artist.dat");
            File  tempArtistFile = new File ("artist.tmp");
            Artist tempArtist = new Artist ();	
            if (!artistFile.exists ())
            {
                return;
            }
            RandomAccessFile inFile = new RandomAccessFile (artistFile, "r");
            RandomAccessFile outFile = new RandomAccessFile (tempArtistFile, "rw");
            while (inFile.getFilePointer () != inFile.length ())
            {
                tempArtist.read (inFile);
                if (!(artistFirstName.equalsIgnoreCase(tempArtist.getArtistFirstName()) &&
                artistLastName.equalsIgnoreCase(tempArtist.getArtistLastName())))	 
                {
                    tempArtist.write (outFile);
                }
            }
            inFile.close ();
            outFile.close ();
            artistFile.delete ();
            tempArtistFile.renameTo (artistFile);
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: BoughtPainting.performDeletion () *****");
            System.out.println ("\t" + e);
        }
    }  

    // Desc: Prints out a single record of an Artist
    // Post: one line is printed to the screen.
     public void print ()
    {
      System.out.print ("Artist First Name: " + artistFirstName);
      System.out.print ("\t Artist Last Name: " + artistLastName);
      System.out.println ("\t Fashionability Value: " + fashionabilityValue);
    } 

   public void add ()

  {
    try
    {
	int c;	// character entered by user

	//obtainNewData ();
       /* updateFashionabilityValue();
        updateArtistLastName();
        updateArtistsFirstName();*/
        readInRecord();
	save ();
	System.out.println ("\nThe following record was inserted\n");
	print ();
	//UserInterface.pressEnter();

    }
    catch (Exception e)
    {
	System.out.println ("***** Error: Artist.add () *****");
	System.out.println ("\t" + e);
    }

  }  // add

}
