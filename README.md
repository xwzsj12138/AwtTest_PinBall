# AwtTest_PinBall
This is a java awt component mini-exercise called pinball mini-game. I hope it will help you all.


import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

    public class PinBall {
        public static void main(String[] args) {
            new PinBall().init();
        }

        private Frame frame = new Frame("弹球游戏");
        //桌面宽,长度度
        private final int TABLE_WIDTH = 300;
        private final int TABLE_HEIGHT = 400;
        //球拍的高度与长度
        private final int RACKET_WIDTH = 60;
        private final int RACKET_HEIGHT = 20;
        //小球的大小
        private final int BALL_SIZE = 16;

        //定义变量，记录小球的坐标
        private int ballX = 120;
        private int ballY = 20;

        //定义变量，记录小球在x，y方向上的移动速度
        private int speedX = 20;
        private int speedY = 10;

        //定义球拍的坐标
        private int racketX = 120;
        private final int racketY = 340;

        //定义变量，定义当前游戏是否结束
        private boolean isOver = false;

        //声明一个定时器
        private Timer timer;

        //自定义一个类，继承Canvas，充当画布
        private class MyCanvas extends Canvas {
            @Override
            public void paint(Graphics g) {
                //在这里绘制内容

                if (isOver) {
                    //游戏结束
                    g.setColor(Color.BLUE);

                    g.setFont(new Font("Times", Font.BOLD, 30));

                    g.drawString("游戏结束", 50, 200);
                } else {
                    //游戏中
                    g.setColor(Color.RED);

                    g.fillOval(ballX, ballY, BALL_SIZE, BALL_SIZE);

                    g.fillRect(racketX, racketY, RACKET_WIDTH, RACKET_HEIGHT);


                }
            }
        }

        public void init() {
            //组装视图,游戏的逻辑控制

            //完成球拍坐标的变化
            KeyListener listener = new KeyAdapter() {
                @Override
                public void keyPressed(KeyEvent e) {

                    //获取当前按下的键
                    int keyCode = e.getKeyCode();

                    if (keyCode == KeyEvent.VK_LEFT) {
                        //应该向左移动
                        if (racketX > 0) {
                            racketX -= 10;
                        }
                    } else if (keyCode == KeyEvent.VK_RIGHT) {
                        //应该向右移动
                        if (racketX < (TABLE_WIDTH - RACKET_WIDTH)) {
                            racketX += 10;
                        }
                    }
                }
            };

            //给FRAME和drawArea注册监听
            frame.addKeyListener(listener);
            drawArea.addKeyListener(listener);

            //小球坐标的控制
            ActionListener task = new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    //更新小球的坐标

                    //根据边界范围，修正速度
                    if (ballX <= 0 || ballX >= (TABLE_WIDTH - BALL_SIZE)) {
                        speedX = -speedX;
                    }
                    if (ballY <= 0 || ballY > racketY - BALL_SIZE && ballX > racketX && ballX < racketX + RACKET_WIDTH) {
                        speedY = -speedY;
                    }
                    if (ballY>racketY-BALL_SIZE&&(ballX<racketX||ballX>racketX+RACKET_WIDTH)){
                        //当前小球超出球拍的范围，游戏结束

                        //停止定时器
                        timer.stop();
                        //修改游戏是否结束的标记
                        isOver=true;
                        //重绘
                        drawArea.repaint();

                    }
                    ballX += speedX;
                    ballY += speedY;
                    //重绘界面
                    drawArea.repaint();
                }
            };
            timer = new Timer(100, task);
            timer.start();

            drawArea.setPreferredSize(new Dimension(TABLE_WIDTH,TABLE_HEIGHT));
            frame.add(drawArea);

            frame.pack();
            frame.setVisible(true);

        }

        //创建绘画区域
        MyCanvas drawArea = new MyCanvas();
    }
