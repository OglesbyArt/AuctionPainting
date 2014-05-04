MasterpieceAuctionPainting
==========================

import java.io.File;
import java.io.RandomAccessFile;
import java.util.Date;


public class MasterpieceAuctionPainting extends Painting
{

    private Date dateOfAuction;
    private double auctionSalesPrice;

    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting 
    MasterpieceAuctionPainting (String fname, String lname, 
                String titleofWork, Date doWork, Date doAuction,
                double aucSalesPrice, String medium)
    {
        artistFirstName=fname;
	artistLastName=lname;
	titleOfWork=titleofWork;
	dateOfWork=doWork;
	dateOfAuction =doAuction;
        auctionSalesPrice = aucSalesPrice;
	medium=medium;
    }
    
    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting 
    MasterpieceAuctionPainting()
    {
        
    }

    //Return: The date of the Auction
    public Date getDateofAuction()
    {
        return dateOfAuction;
    }
    
    //Desc: Set date of auction to d
    public void setDateofAuction(Date d)
    {
        dateOfAuction = d;
    }
	//Desc: Set Auction Sales Price to d
    public void setAuctionSalesPrice(double d)
    {
        auctionSalesPrice = d;
    }

    //Return: The auction sales price for the painting
    public double getAuctionSalesPrice()
    {
    	return auctionSalesPrice;
    }

    //Desc: uses the last name of an artist and the title of work to find the
    //       Bought Painting object in the array 
    //Return: returns the found Artist object or null value if Artist not found
    public boolean find(String alastname, String title) //should only appear in painting
    {
        try
        {
            File paintingsFile = new File ("AuctionPaintings.dat");
            boolean found = false;      

            if (paintingsFile.exists ())
            {
                RandomAccessFile inFile = new RandomAccessFile (paintingsFile, "r");

                while (!found && (inFile.getFilePointer () != inFile.length ()))
                {
                    read (inFile);

                    if (artistLastName.equals(alastname) && titleOfWork.equals(title))
                        found = true;
                }
                inFile.close();
            }
            return found;
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: MasterpieceAuctionPainting.find () *****");
            System.out.println ("\t" + e);

            return false; //returns boolean right now, not artist
        }
    }
    
    //Desc: reads a file into the scanner, sets each field in the text file to 
    //      a field in a new Bought Painting object
    //Pre: file must exist
    public void read(RandomAccessFile fileName)
    {
        try
        {
            String  inputString = new String ();    // for storing artist record
            int i = 0;                      // position in record
            inputString = fileName.readLine ();
            StringBuffer input = new StringBuffer ();   // for storing field within record
            while (inputString.charAt (i) != '|')
            {
                input.append (inputString.charAt (i));
                i++;
            }
            artistFirstName = input.toString ();
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
            titleOfWork = input.toString ();
            i++;

            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
                input.append (inputString.charAt (i));
                i++; 
            }
            Date tempdow = new Date (input.toString ());
            dateOfWork = tempdow;
            i++;

            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
                input.append (inputString.charAt (i));
                i++; 
            }
            Date tempdoa = new Date (input.toString ());
            dateOfAuction = tempdoa;
            i++;


            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
                input.append (inputString.charAt (i));
                i++;
            }
            Double tempprice = new Double (input.toString ());
            auctionSalesPrice = tempprice;
            i++;
    }
    catch (Exception e)
    {
        System.out.println ("***** Error: MasterpieceAuctionPainting.read () *****");
        System.out.println ("\t" + e);
    }

  }
//Desc: writes the variables dateOfPurchase, nameOfSeller, addressOfSeller, 
//      actualPurchasePrice, and targetSellingPrice to a record in the file
//Post: updates the specified file
public void write(RandomAccessFile fileName)
{
        try
        {
            fileName.writeChars(artistFirstName + "|" + artistLastName + 
                "|" + titleOfWork+ "|" + dateOfWork + "|"+ 
                dateOfAuction + "|" + auctionSalesPrice + "\n");
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: MasterpieceAuctionPainting.write () *****");
            System.out.println ("\t" + e);
        }
}
//Desc: writes the record back to the text file and prompts the 
//      user the data has been saved
//Post: changes the text file
public void save()
{
    try
    {
        File paintingsFile = new File ("AuctionPaintings.dat"); // file of artist
        File  temppaintingsFile = new File ("AuctPaintings.tmp"); // temporary file for artist

        BoughtPainting tempBoughtPainting = new BoughtPainting ();  // record read, then written
        boolean found = false;      // terminates while-loop

        RandomAccessFile newFile = new RandomAccessFile (temppaintingsFile, "rw");

        if (!paintingsFile.exists ())
        {
          write(newFile);
        }
        else
        {
            RandomAccessFile oldFile = new RandomAccessFile (paintingsFile, "r");

            int comparePaintings; // to find correct place for the new investment

            while (oldFile.getFilePointer () != oldFile.length ()) //the pointer hasn't reached the end of the file
            {
                tempBoughtPainting.read(oldFile); //read walks through each field in the record

                if (artistLastName.equals(tempBoughtPainting.getArtistLastName())
                    && titleOfWork.equals(tempBoughtPainting.getTitleofWork()))
                    comparePaintings=1;
                else comparePaintings=0;

                if (comparePaintings ==1) 
                {
                    write (newFile); //replaces old file record with new file record
                } 
                else
                {
                  tempBoughtPainting.write (newFile); //writes to 2nd temp file
                }
          }  // while

          //if (compareArist==0) write (newFile); // if never found in file, write to temp file

          oldFile.close ();

        }  // else

        newFile.close ();

        paintingsFile.delete ();
        temppaintingsFile.renameTo (paintingsFile);
        System.out.println("record saved");

      }
      catch (Exception e)
      {
          System.out.println ("***** Error: MasterpieceAuctionPainting.putRecord () *****");
          System.out.println ("\t" + e);
      }
    }
}
