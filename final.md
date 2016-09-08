# ACSL-Isola

//Andrea Shulman
//Contest 3
//3/6/15

import java.util.*;
public class Isola
{
   private int board[][];
   private int blocked[];
   private ArrayList<Integer> pos;//+ possible moves
   private ArrayList<Integer> pos2;//x possible moves
   private ArrayList<Integer> inter;//where pos adn pos2 intersect possible moves
   private ArrayList<Integer> print;//final list
   private int tPos;
   private int xPos;
   private int tRow, tCol, xRow, xCol;
   private ArrayList<Integer> surr;
   
   public Isola(String line)
   {
      pos=new ArrayList<Integer>();
      pos2=new ArrayList<Integer>();
      inter=new ArrayList<Integer>();
      surr=new ArrayList<Integer>();
      print=new ArrayList<Integer>();
      stringProcessor(line);
      int counter=1;
      tRow=0;
      tCol=0;
      board= new int[7][7];
      boolean flag=false;
      for(int row=6;row>=0;row--){
         for(int col=0; col<7;col++){
            for(int isBlocked=0;isBlocked<blocked.length;isBlocked++)
               if(blocked[isBlocked]==counter)
                  flag=true;
            if(flag)
               board[row][col]=0;
            else if(counter==tPos)
            {
               tRow=row;
               tCol=col;
            }
            else if(counter==xPos)
            {
               xRow=row;
               xCol=col;
            }
            else
               board[row][col]=counter;
            counter++;
            flag=false;
         }
      }//board is set up
      findTPos();
      findXPos();
      findInter();
      findSurr();
      newList();
   }
   
   public void findTPos()//finds all positions available to move t
   {
      for(int i=tRow;i>0;i--)//down
         if(board[i-1][tCol]!=0)
            pos.add(board[i-1][tCol]);
         else
           i=0;
      for(int j=tCol;j>0;j--)//left
         if(board[tRow][j-1]!=0)
            pos.add(board[tRow][j-1]);
         else
            j=0;
      for(int f=tRow;f<6;f++)//up
         if(board[f+1][tCol]!=0)
            pos.add(board[f+1][tCol]);
         else
            f=6;
      for(int r=tCol;r<6;r++)//right
         if(board[tRow][r+1]!=0)
            pos.add(board[tRow][r+1]);
         else
            r=6;
      for(int f=1;f<tCol;f++)//left/up diag
         if(tRow-f>=0 && tCol-f>=0){
            if(board[tRow-f][tCol-f]!=0)   
               pos.add(board[tRow-f][tCol-f]);
            else
               f=tCol;
         }    
      for(int f=1;f<tRow;f++)//right/up diag
         if(tRow-f>=0 && tCol+f<=6){
            if(board[tRow-f][tCol+f]!=0)   
               pos.add(board[tRow-f][tCol+f]);
            else
               f=tRow;
         }
      for(int f=1;f<tCol;f++)//left/down diag
         if(tRow+f<=6 && tCol-f>=0){
           if(board[tRow+f][tCol-f]!=0)   
               pos.add(board[tRow+f][tCol-f]);
           else
               f=tCol;
         }
      for(int f=1;f<tRow;f++)//right/down diag
         if(tRow+f<=6 && tCol+f<=6){
            if(board[tRow+f][tCol+f]!=0)   
               pos.add(board[tRow+f][tCol+f]);
            else
               f=tRow;
         }
      for(int k=0;k<pos.size();k++)//removes x
         if(pos.get(k)==xPos)
            pos.remove(k);
      //for(int j=0;j<pos.size();j++)
        // System.out.println("-"+pos.get(j));
   }
   
   public void newList()
   {
      /*for(int i=0;i<surr.size();i++)
         System.out.println(surr.get(i));
      for(int j=0;j<inter.size();j++)
         System.out.println("inter:"+inter.get(j));
      for(int j=0;j<pos2.size();j++)
         System.out.println("pos2"+pos2.get(j));*/
      
      //----------prelim----------------
      //if x never hits a surrounding then print none
      int num=0;
      int hold=0;
      boolean flag=false;
      for(int i=0;i<surr.size();i++){
         hold=i;
         for(int j=0;j<inter.size();j++)
            if(surr.get(i)==inter.get(j)){
               //System.out.println("hi");
               flag=true;
               num=j;
            }
         for(int j=0;j<pos2.size();j++)
            if(surr.get(hold)==pos2.get(j)){
               //System.out.println("hi");
               flag=true;
               num=j;
            }
      }
      if(!flag){
         System.out.println("NONE");
         return;
      }         
      //-------------prelim-------------------
      
      for(int i=0;i<surr.size();i++)
        for(int j=0;j<pos2.size();j++)
           if(surr.get(i)==pos2.get(j))
           {
               //System.out.println(i+"check"+j);
               for(int k=j;k>=0;k--)
                  if(pos2.get(k)!=0)
                     print.add(pos2.get(k));
                  else
                     k=0;
               for(int h=j;h<pos2.size();h++)
                  if(pos2.get(h)!=0)
                     print.add(pos2.get(h));
                  else
                     h=pos2.size();
           }   
      HashSet hs=new HashSet();
      hs.addAll(print);
      print.clear();
      print.addAll(hs);
      String ret="";             
      for(int k=0;k<print.size();k++)
         ret+=print.get(k)+",";
      System.out.println(ret.substring(0,ret.length()-1));
   }
   
   public void findXPos()//finds all positions for x with a 0 between directions
   {
      ArrayList<Integer> temp=new ArrayList<Integer>();
      //System.out.println(xPos);
      for(int i=xRow;i>0;i--)//up
         if(board[i-1][xCol]!=0)
            temp.add(board[i-1][xCol]);
         else
            i=0;
      temp.add(0);
      for(int j=xCol;j>0;j--)//left
         if(board[xRow][j-1]!=0)
            temp.add(board[xRow][j-1]);
         else
            j=0;
      temp.add(0);
      for(int f=xRow;f<6;f++)//down
         if(board[f+1][xCol]!=0)
            temp.add(board[f+1][xCol]);
         else
            f=6;
      temp.add(0);
      for(int r=xCol;r<6;r++)//right
         if(board[xRow][r+1]!=0)
            temp.add(board[xRow][r+1]);
         else
            r=6;
      temp.add(0);
      for(int f=1;f<xRow;f++)//left/up diag
         if(xRow-f>=0 && xCol-f>=0){
            if(board[xRow-f][xCol-f]!=0)   
               temp.add(board[xRow-f][xCol-f]);
            else
               f=xCol;
         }
      temp.add(0); 
      for(int f=1;f<xRow;f++)//right/up diag
         if(xRow-f>=0 && xCol+f<=6){
            if(board[xRow-f][xCol+f]!=0)   
               temp.add(board[xRow-f][xCol+f]);
            else
               f=xRow;
         }
      temp.add(0);
      for(int f=1;f<xCol;f++)//left/down diag
         if(xRow+f<=6 && xCol-f>=0){
            if(board[xRow+f][xCol-f]!=0)   
               temp.add(board[xRow+f][xCol-f]);
            else
               f=xCol;
         }
      temp.add(0);
      for(int f=1;f<xRow;f++)//right/down diag
         if(xRow+f<=6 && xCol+f<=6){
            if(board[xRow+f][xCol+f]!=0)   
               temp.add(board[xRow+f][xCol+f]);
            else
               f=xRow;
         }
      temp.add(0);
      for(int k=0;k<temp.size();k++)//removes t
         if(temp.get(k)==tPos)
            temp.remove(k);      
      pos2=temp;
   }
   
   public void findInter()
   {
      //for(int i=0;i<pos.size();i++)
        // System.out.println("-"+pos.get(i));
      for(int i=0;i<pos.size();i++)
         for(int j=0;j<pos2.size();j++)
            if(pos.get(i)==pos2.get(j)&&pos.get(i)!=0)
               inter.add(pos.get(i));
   }
   
   public void findSurr()//finds the positions that are not blocked around t
   {
      if(tRow-1>=0 && board[tRow-1][tCol]!=0)
         surr.add(board[tRow-1][tCol]);
      if(tCol-1>=0 && board[tRow][tCol-1]!=0)
            surr.add(board[tRow][tCol-1]);
      if(tRow+1<=6 && board[tRow+1][tCol]!=0)
            surr.add(board[tRow+1][tCol]);
      if(tCol+1<=6 && board[tRow][tCol+1]!=0)
            surr.add(board[tRow][tCol+1]);
      if(tRow-1>=0 && tCol-1>=0)
            if(board[tRow-1][tCol-1]!=0)   
               surr.add(board[tRow-1][tCol-1]);
      if(tRow-1>=0 && tCol+1<=6)
            if(board[tRow-1][tCol+1]!=0)   
               surr.add(board[tRow-1][tCol+1]);
      if(tRow+1<=6 && tCol-1>=0)
            if(board[tRow+1][tCol-1]!=0)   
               surr.add(board[tRow+1][tCol-1]);
      if(tRow+1<=6 && tCol+1<=6)
            if(board[tRow+1][tCol+1]!=0)   
               surr.add(board[tRow+1][tCol+1]);
   }
   
   public void stringProcessor(String str)
   {
      ArrayList<Integer> commas=new ArrayList<Integer>(); //array with indexes of commas
      int count=0;
      
      for(int i=0; i<str.length();i++)
         if((str.substring(i,i+1)).equals(","))
         {
            commas.add(i);
            count++;
         }
      count=0;
      tPos=Integer.parseInt(str.substring(0,(commas.get(0))));
      xPos=Integer.parseInt(str.substring(commas.get(0)+1,commas.get(1)));
      blocked=new int [commas.size()-2];
      for(int j=1;j<commas.size()-1;j++)
      {
         blocked[count]=Integer.parseInt(str.substring(commas.get(j)+1,commas.get(j+1)));
         count++;
      }
   }
   
   public static void main (String[]args)
   {
      /*Isola test1=new Isola("46,4,45,47,38,40,0");
      Isola test2=new Isola("49,17,48,42,0");
      Isola test3=new Isola("34,4,40,41,35,28,0");
      Isola test4=new Isola("30,11,36,37,38,31,24,22,0");
      Isola test5=new Isola("7,5,14,6,0");*/
      
      Isola test1=new Isola("43,25,44,36,0");
      Isola test2=new Isola("22,28,29,30,15,16,0");
      Isola test3=new Isola("46,22,45,47,40,39,0");
      Isola test4=new Isola("28,27,34,35,21,14,0");
      Isola test5=new Isola("24,22,31,25,17,0");
   }
}
