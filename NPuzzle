   //Created by: Hedgehog
   //Date: April 9, 2012
   
   import java.awt.*;
   import javax.swing.*;
   import java.awt.event.*;
   import java.util.*;
  
    public class NPuzzle
   {
       public static void main(String[] args)
      {
         JFrame board = new JFrame("N-Puzzle");
         board.setSize(400, 400);
         board.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
         board.setContentPane(new Puzzle());
         board.setVisible(true);
      }
   }

    class Puzzle extends JPanel
   {
      private JButton[] squares;
      private JButton pointerReal;
      private JButton pointerNull;
      private int pReal;
      private int pNull;
      private int len;
      private int steps;
      private long millitime;
      private boolean startTimer;
       public Puzzle()
      {
         String leng = "";
         try
         {
            addKeyListener(new ArrowKeyControl());
            setFocusable(true);
         
            steps = 0;
            startTimer = false;
            leng = JOptionPane.showInputDialog("Side length? 2-24");
            len = Integer.valueOf(leng);
            if(len < 2 || len > 24)
            {
               JOptionPane.showMessageDialog(null, "Invalid Dimension Size");
               System.exit(0);
            }
            setLayout(new GridLayout(len, len));
            squares = new JButton[len * len];
            
            do
               randomizeBoard();
            while(checkInversions());
            
            createBoard();
            repaint();
         }
             catch(Exception x)
            {
               try
               {
                  if(leng.equals(""))
                     JOptionPane.showMessageDialog(null, "Thanks For Playing!");
                  else
                     JOptionPane.showMessageDialog(null, "That Is Not A Number");
               }
                   catch(Exception q)
                  {
                     if(q instanceof NullPointerException)
                        JOptionPane.showMessageDialog(null, "Thanks For Playing!");
                     else
                        JOptionPane.showMessageDialog(null, "An Error Has Occurred?");
                     System.exit(0);
                  }
               
               System.exit(0);
            }
      }
   
       public void createBoard()
      {
         for(int i = 0; i < squares.length; ++i)
         {
            squares[i].addActionListener(new MouseControl());
            squares[i].setBackground(Color.WHITE);
            add(squares[i]);
         }
      }
   
       public void randomizeBoard()
      {
         ArrayList<Integer> list = new ArrayList<Integer>();
         int skip = (int)(Math.random() * len * len);
         for(int i = 1; i < squares.length; ++i)
            list.add(i);
         for(int i = 0; i < squares.length; ++i)
         {
            if(i == skip)
            {
               squares[i] = new JButton("");
               pointerNull = squares[i];
               pNull = i;
            }
            else
               squares[i] = new JButton("" + list.remove((int)(Math.random() * list.size())));
         }
      }
   	
       public boolean checkInversions()
      {
         int inversions = 0;
         for(int i = 0; i < squares.length - 1; ++i)
         {
            if(i == pNull)
               continue;
            for(int k = i + 1; k < squares.length; ++k)
            {
               if(k == pNull)
                  continue;
               if(Integer.parseInt(squares[i].getText()) > Integer.parseInt(squares[k].getText()))
                  inversions++;
            }
         }
         
         if(len % 2 == 1)
            return inversions % 2 == 1;
         if(row(pNull) % 2 == 1 && inversions % 2 == 0)
            return false;
         if(row(pNull) % 2 == 0 && inversions % 2 == 1)
            return false;
         return true;
      }
   
       public boolean solved()
      {
         for(int i = 0; i < squares.length - 1; ++i)
            if(squares[i].getText() == "")
               return false;
            else if(Integer.parseInt(squares[i].getText()) != i + 1)
               return false;
         return true;
      }
      
       public void switchSquare(String value)
      {
         pointerNull.setText(value);
         pointerReal.setText("");
         pointerNull = pointerReal;
         int temp = pNull;
         pNull = pReal;
         pReal = temp;
         
         // for(int i = 0; i < squares.length; ++i)
            // System.out.print(squares[i].getText() + " ");
         //System.out.println();
      	//System.out.println(heuristic());
      }
      
       public void finish()
      {
         squares[squares.length - 1].setText(Integer.toString(squares.length));
         squares[squares.length - 1].setBackground(Color.YELLOW);
         String completion = "SOLVED - " + len + "X" + len;
         completion += "\nSteps: " + steps;
         completion += "\nSeconds: " + (System.currentTimeMillis() - millitime) / 1000.0;
         JOptionPane.showMessageDialog(null, completion);
         System.exit(0);
      }
      
       public boolean sameRow(int square1, int square2)
      {
         return square1 / len == square2 / len;
      }
      
       public boolean sameColumn(int square1, int square2)
      {
         return Math.abs(square1 - square2) == len;
      }
      
       public boolean adjacent(int square1, int square2)
      {
         return Math.abs(square1 - square2) == 1;
      }
      
       public int row(int square)
      {
         return square / len;
      }
      
       public int heuristic()
      {
         int dislodged = 0;
         for(int i = 0; i < squares.length; ++i)
            if(Integer.parseInt(squares[i].getText()) == i + 1)
               dislodged++;
         return dislodged;
      }
    
       private class MouseControl implements ActionListener
      {
          public void actionPerformed(ActionEvent e)
         {
            String value = e.getActionCommand();
            for(int i = 0; i < squares.length; ++i)
               if(squares[i].getText().equals(value))
               {
                  pointerReal = squares[i];
                  pReal = i;
                  break;
               }
            if(sameColumn(pReal, pNull) || adjacent(pNull, pReal) && sameRow(pNull, pReal))
            {
               if(!startTimer)
               {
                  millitime = System.currentTimeMillis();
                  startTimer = true;
               }
               switchSquare(value);
               steps++;
            }
         
            if(solved())
               finish();
         }
      }
   	
       private class ArrowKeyControl extends KeyAdapter
      {
          public void keyPressed(KeyEvent e)
         {
            boolean moved = false;
            int pServ = pReal;
            JButton pointerServ = pointerReal;
            try
            {
               if(e.getKeyCode() == KeyEvent.VK_UP)
               {
                  pReal = pNull + len;
                  pointerReal = squares[pReal];
                  String value = pointerReal.getText();
                  switchSquare(value);
                  moved = true;
               }
               if(e.getKeyCode() == KeyEvent.VK_DOWN)
               {
                  pReal = pNull - len;
                  pointerReal = squares[pReal];
                  String value = pointerReal.getText();
                  switchSquare(value);
                  moved = true;
               }
               if(e.getKeyCode() == KeyEvent.VK_LEFT)
               {
                  pReal = pNull + 1;
                  if(sameRow(pReal, pNull))
                  {
                     pointerReal = squares[pReal];
                     String value = pointerReal.getText();
                     switchSquare(value);
                     moved = true;
                  }
                  else
                     pReal--;
               }
               if(e.getKeyCode() == KeyEvent.VK_RIGHT)
               {
                  pReal = pNull - 1;
                  if(sameRow(pReal, pNull))
                  {
                     pointerReal = squares[pReal];
                     String value = pointerReal.getText();
                     switchSquare(value);
                     moved = true;
                  }
                  else
                     pReal++;
               }
               
               if(moved)
               {
                  steps++;
                  if(!startTimer)
                  {
                     millitime = System.currentTimeMillis();
                     startTimer = true;
                  }
               }
            	
               if(solved())
                  finish();
            }
                catch(Exception x)
               {
                  pReal = pServ;
                  pointerReal = pointerServ;
               }
         }
      }
   }
