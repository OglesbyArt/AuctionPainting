
import java.io.File;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.Date;


public class AuctionPainting extends Painting
{

    private Date dateOfAuction;
    private double auctionSalesPrice;

    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting
    AuctionPainting (String fname, String lname,
                String titleofWork, Date doWork, Date doAuction,
                double aucSalesPrice, String med)
    {
        artistFirstName=fname;
	artistLastName=lname;
	titleOfWork=titleofWork;
	dateOfWork=doWork;
	dateOfAuction =doAuction;
        auctionSalesPrice = aucSalesPrice;
	medium=med;
    }

    //Desc: constructor for MasterpieceAuctionPainting
    //Post: Creates a new MasterpieceAuctionPainting
    AuctionPainting()
    {
        artistFirstName="";
	artistLastName="";
	titleOfWork="";
	dateOfWork=new Date();
	dateOfAuction =new Date();
        auctionSalesPrice =0;
	medium="";
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

    //Desc: updates the dateOfPurchase in a record from user's input
    //Post: dateOfPurchase field is updated
    public void updateDateofAuction()
    {
        try
        {
            System.out.println("Old Date of Purchase:" + dateOfAuction);
            System.out.println("Please enter new Date of Purchase (mm/dd/yyyy) and press <ENTER>: ");
            Date tempDate=new Date(UserInterface.getString());
            dateOfAuction=tempDate;
        }

        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not a date value. Please enter a date value (mm/dd/yyyy): ");
           Date tempDate=new Date (UserInterface.getString());
           dateOfAuction=tempDate;
           return;
        }
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

        //Desc: updates the dateOfPurchase in a record from user's input
    //Post: dateOfPurchase field is updated
    public void updateAuctionSalesPrice()
    {
        try
        {
            System.out.println("Old Target Selling Price:" + auctionSalesPrice);
            System.out.println("Please enter new Target Selling Price and press <ENTER>: ");
            double tempprice=new Double(UserInterface.getString());
            auctionSalesPrice=tempprice;
        }

        catch (NumberFormatException e)
        {
           System.out.println("Value entered is not an double value. Please enter a double value: ");
           auctionSalesPrice=Double.parseDouble(UserInterface.getString());
           return;
        }
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
            System.out.println ("***** Error: AuctionPainting.find () *****");
            System.out.println ("\t" + e);

            return false; //returns boolean right now, not artist
        }
    }

//Desc: reads an MasterpieceAuctionPainting record form fileName
//Pre: file must exist
//Post: all fields in this object are changed
public void read(RandomAccessFile fileName)
{
    try
    {
            String  inputString = new String ();
            int	i = 0;
            inputString = fileName.readLine ();

            //Artist First Name
            StringBuffer input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistFirstName = input.toString ();
            i++;

            //Artist Last Name
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            artistLastName = input.toString ();
            i++;

            //Title of Work
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
             titleOfWork = input.toString ();
            i++;

            //Date of Work
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            Date tempdow = new Date (input.toString ());
            dateOfWork = tempdow;
            i++;

            //classification
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            classification = input.toString ();
            i++;

            //Height
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            Double tempheight = new Double (input.toString ());
            height = tempheight;
            i++;

            //Width
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            Double tempwidth = new Double (input.toString ());
            width = tempwidth;
            i++;

            //Medium
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            medium = input.toString();
            i++;

            //Subject
            input = new StringBuffer ();
            while (inputString.charAt (i) != '|')
            {
              input.append (inputString.charAt (i));
              i++;
            }
            subject = input.toString();
            i++;


        input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Date tempDateA = new Date(input.toString());
        dateOfAuction=tempDateA;
        i++;
        
	//Auction Sales Price
	input = new StringBuffer ();
        while (inputString.charAt (i) != '|')
        {
          input.append (inputString.charAt (i));
          i++;
        }
        Double price = new Double(input.toString());
        auctionSalesPrice = price;
        i++;

    }
    catch (Exception e)
    {
        System.out.println ("***** Error: AuctionPainting.read () *****");
        System.out.println ("\t" + e);
    }

}

//Desc: writes the fields of the object to a record line in the specified file
//Post: updates the specified file
public void write(RandomAccessFile fileName)
{
    try
    {
        fileName.writeBytes(artistFirstName + "|");
        fileName.writeBytes(artistLastName + "|");
        fileName.writeBytes(titleOfWork + "|");
        fileName.writeBytes(dateOfWork + "|");
        fileName.writeBytes(classification + "|");
        fileName.writeBytes(height + "|");
        fileName.writeBytes(width + "|");
        fileName.writeBytes(medium + "|");
        fileName.writeBytes(subject + "|");
        String dateOfA="";
        dateOfA=dateOfA.valueOf(dateOfAuction);
        fileName.writeBytes(dateOfA+"|");
        String price="";
        price=price.valueOf(auctionSalesPrice);
        fileName.writeBytes(price+"|");

    }
    catch (IOException e)
    {
        System.out.println ("***** Error: AuctionPainting.write () *****");
        System.out.println ("\t" + e);
    }
}

    //Desc: saves an individual masterpieceAuctionPainting record into a file
    //Post: the message informing the user has been printed
    public void save()
    {
        try
        {
            File auctFile = new File ("AuctionPainting.dat");
            File  tempAuctFile = new File ("AuctionPainting.tmp");
            AuctionPainting tempAuct = new AuctionPainting ();
            boolean found = false;
            RandomAccessFile newFile = new RandomAccessFile (tempAuctFile, "rw");

            if (!auctFile.exists ())
            {
              write(newFile);
            }
            else
            {
                RandomAccessFile oldFile = new RandomAccessFile (auctFile, "r");
                boolean compareAuct;
                while (oldFile.getFilePointer () != oldFile.length ())
                {
                    tempAuct.read(oldFile);
                    if (artistFirstName.equalsIgnoreCase(tempAuct.getArtistsFirstName()) &&
                    titleOfWork.equalsIgnoreCase(tempAuct.getTitleofWork()))
                        compareAuct=true;
                    else compareAuct=false;
                    if(compareAuct)
                    {
                        write (newFile);
                        found=true;
                    }
                    else
                    {
                      tempAuct.write(newFile);
                    }
                }
                if (!found) write (newFile); // if never found in file, write to temp file
                oldFile.close ();
            }
            newFile.close ();
            auctFile.delete ();
            tempAuctFile.renameTo (auctFile);
            System.out.println("record saved to file");
        }
        catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.putRecord () *****");
            System.out.println ("\t" + e);
        }
    }
    public void print ()
    {
        System.out.print ("Artist First Name: " + artistFirstName);
        System.out.print ("\t Artist Last Name: " + artistLastName);
        System.out.print ("\t Title Of Work: " + titleOfWork);
        System.out.print ("\t Date Of Work: " + dateOfWork);
        System.out.print ("\t Classification: " + classification);
        System.out.print ("\t Height: " + height);
        System.out.print ("\t Width: " + width);
        System.out.print ("\t Medium: " + medium);
        System.out.print ("\t Subject: " + subject);

        System.out.println ("\t Date of Auction: " + dateOfAuction);
        System.out.println ("\t Auction Sales Price: " + auctionSalesPrice);

    }

    //Desc: reads in all fields into a Painting object from user input
    //Post: all fields in Auction Painting are modified
    public void readInRecord()
    {
        try
        {
            System.out.println("Enter Artist First name: ");
            artistFirstName = UserInterface.getString();

            System.out.println("Enter Artist Last name: ");
            artistLastName= UserInterface.getString();

            System.out.println("Enter title of painting: ");
            titleOfWork = UserInterface.getString();

            System.out.println("Enter classification of Painting (masterpiece, masterwork, or other): ");
            classification = UserInterface.getString();
            while (!(classification.equalsIgnoreCase("masterpiece")|classification.equalsIgnoreCase("masterwork")|
                    classification.equalsIgnoreCase("other")))
            {
                System.out.println("Classification entered incorrectly. Please enter one of the following mediums: masterpiece, masterwork, or other.");
                classification=UserInterface.getString();
            }

            System.out.println("Enter the date the painting was created (mm/dd/yyyy): ");
            Date tempdate = new Date(UserInterface.getString());
            dateOfWork=tempdate;

            System.out.println("Enter painting medium (oil, watercolor, or other): ");
            medium  = UserInterface.getString();
            while (!(medium.equalsIgnoreCase("oil")|medium.equalsIgnoreCase("watercolor")|
                    medium.equalsIgnoreCase("other")))
            {
                System.out.println("Medium entered incorrectly. Please enter one of the following mediums: oil, watercolor, or other.");
                medium=UserInterface.getString();
            }

            System.out.println("Enter painting subject (portrait, still-life, landscape, or other): ");
            subject =UserInterface.getString();
            while (!(subject.equalsIgnoreCase("portrait")|subject.equalsIgnoreCase("still-life")|
            subject.equalsIgnoreCase("landscape")| subject.equalsIgnoreCase("other")))
            {
                System.out.println("Subject entered incorrectly. Please enter one of the following subjects: portrait, still-life, landscape, or other.");
                subject=UserInterface.getString();
            }

            System.out.println("Enter painting width: ");
            Double tempw=new Double( UserInterface.getString());
            width =tempw;

            System.out.println("Enter painting height: ");
            Double temph=new Double(UserInterface.getString());
            height =temph;

           System.out.println("Enter the date the painting was bought at auction (mm/dd/yyyy): ");
            tempdate = new Date(UserInterface.getString());
            dateOfAuction=tempdate;


            System.out.println("Enter painting's auction Sale Price: ");
            tempw=new Double( UserInterface.getString());
            auctionSalesPrice =tempw;
        }

        catch (Exception e)
        {
           System.out.println("Record value entered is not the correct data type. Please re-enter the record: ");
           readInRecord();
        }
        /* catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.readInRecord () *****");
            System.out.println ("\t" + e);
        }*/
    }

    //Desc: Deletes a single record in the GalleryPaintings.dat File if the artist last
    //      name and first name matches this object.
    //Post: Modifies the GalleryPaintings.dat File
    public void performDeletion ()
    {
        try
        {
            File  paintingsFile = new File ("AuctionPainting.dat");
            File  tempPaintingsFile = new File ("AuctionPainting.tmp");

            AuctionPainting ap = new AuctionPainting ();	// record to be checked

            if (!paintingsFile.exists ())
            {
              return;
            }

            RandomAccessFile inFile = new RandomAccessFile (paintingsFile, "r");
            RandomAccessFile outFile = new RandomAccessFile (tempPaintingsFile, "rw");

            while (inFile.getFilePointer () != inFile.length ())
            {
              ap.read (inFile);

              if (!(artistLastName.equalsIgnoreCase(ap.getArtistLastName()) &&
                        titleOfWork.equalsIgnoreCase(ap.getTitleofWork())))
              {
                  ap.write (outFile);
              }
            }

            inFile.close ();
            outFile.close ();

            paintingsFile.delete ();
            tempPaintingsFile.renameTo (paintingsFile);

        }
        catch (Exception e)
        {
            System.out.println ("***** Error: AuctionPainting.performDeletion () *****");
            System.out.println ("\t" + e);
        }
    }


}
