# java-game
a java game, use:
  javax.swing.*
  java.awt.*
  java.awt.event.ActionEvent
  java.awt.event.ActionListener
  java.awt.event.KeyEvent
  java.awt.event.KeyListener
  java.util.Random
  
my code:
`
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.Random;

public class CatchTheBallGame {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            GameFrame frame = new GameFrame();
            frame.setVisible(true);
        });
    }
}

class GameFrame extends JFrame {
    public GameFrame() {
        setTitle("Catch the Ball Game");
        setSize(600, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setResizable(false);

        GamePanel gamePanel = new GamePanel();
        add(gamePanel);
    }
}

class GamePanel extends JPanel implements ActionListener, KeyListener {
    private final Timer timer;
    private int ballX = 300, ballY = 0, ballSize = new Random().nextInt(20, 40);
    private int paddleX = 250, paddleWidth = 60, paddleHeight = 20;
    private int score = 0;

    public GamePanel() {
        setFocusable(true);
        addKeyListener(this);
        timer = new Timer(15, this); // The game updates every 15ms
        timer.start();
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);

        // Background
        g.setColor(Color.BLACK);
        g.fillRect(0, 0, getWidth(), getHeight());

        // Ball
        g.setColor(Color.RED);
        g.fillOval(ballX, ballY, ballSize, ballSize);

        // Paddle
        g.setColor(Color.BLUE);
        g.fillRect(paddleX, getHeight() - paddleHeight - 20, paddleWidth, paddleHeight);

        // Score
        g.setColor(Color.WHITE);
        g.setFont(new Font("Arial", Font.BOLD, 20));
        g.drawString("Score: " + score, 10, 20);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        ballY += 5; // Move the ball downwards

        // Check if the ball hits the paddle
        if (ballY + ballSize >= getHeight() - paddleHeight - 20 &&
                ballX + ballSize >= paddleX &&
                ballX <= paddleX + paddleWidth) {
            ballY = 0; // Reset ball position
            ballX = (int) (Math.random() * (getWidth() - ballSize));
            score++;
        }

        // Check if the ball falls out of bounds
        if (ballY > getHeight()) {
            timer.stop();
            JOptionPane.showMessageDialog(this, "Game Over! Final Score: " + score);
            System.exit(0);
        }

        repaint();
    }

    @Override
    public void keyPressed(KeyEvent e) {
        int key = e.getKeyCode();

        // Move paddle left
        if (key == KeyEvent.VK_LEFT && paddleX > 0) {
            paddleX -= 15;
        }

        // Move paddle right
        if (key == KeyEvent.VK_RIGHT && paddleX < getWidth() - paddleWidth) {
            paddleX += 15;
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {}

    @Override
    public void keyTyped(KeyEvent e) {}
}

`


