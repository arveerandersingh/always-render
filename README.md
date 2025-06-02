# always-render
always render with us
publish this program
import sys
from PyQt5.QtCore import Qt, QTimer
from PyQt5.QtWidgets import QApplication, QMainWindow, QOpenGLWidget
from OpenGL.GL import *
from OpenGL.GLUT import *
class GLWidget(QOpenGLWidget):
    def init(self, parent=None):
        super(GLWidget, self).init(parent)
        self.timer = QTimer(self)
        self.timer.timeout.connect(self.update)
        self.timer.start(16)  # ~60 FPS
        self.rotation = 0
        self.lastPos = None
    def initializeGL(self):
        glClearColor(0.0, 0.0, 0.0, 1.0)
        glEnable(GL_DEPTH_TEST)
    def resizeGL(self, width, height):
        glViewport(0, 0, width, height)
        glMatrixMode(GL_PROJECTION)
        glLoadIdentity()
        gluPerspective(45, width / height, 0.1, 100.0)
        glMatrixMode(GL_MODELVIEW)
    def paintGL(self):
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glLoadIdentity()
        glTranslatef(0.0, 0.0, -5.0)
        glRotatef(self.rotation, 1.0, 1.0, 1.0)
        glutSolidCube(2.0)
        self.rotation += 1.0
    def mousePressEvent(self, event):
        self.lastPos = event.pos()
    def mouseMoveEvent(self, event):
        dx = event.x() - self.lastPos.x()
        dy = event.y() - self.lastPos.y()
        self.lastPos = event.pos()
        self.rotation += dx
        self.update()
class MainWindow(QMainWindow):
    def init(self):
        super(MainWindow, self).init()
        self.glWidget = GLWidget(self)
        self.setCentralWidget(self.glWidget)
        self.setWindowTitle("Architect Rendering Software")
        self.resize(800, 600)
if name == "main":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
