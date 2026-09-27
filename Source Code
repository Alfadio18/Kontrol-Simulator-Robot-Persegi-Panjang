import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
import math

class RectangleMoverNode(Node):
    def __init__(self):
        super().__init__('rectangle_mover_node')
        
        self.publisher_ = self.create_publisher(Twist, 'cmd_vel', 10)
        
        self.linear_speed = 0.3
        self.angular_speed = 0.4

        self.scale_long = 1.0
        self.scale_short = 1.0
        self.scale_turn = 1.0

        self.t_long = (3.0 / self.linear_speed) * self.scale_long
        self.t_short = (1.5 / self.linear_speed) * self.scale_short
        self.t_turn = ((math.pi / 2) / self.angular_speed) * self.scale_turn  

        self.steps = [
            (self.linear_speed, 0.0, self.t_long, '1. Maju Sisi Panjang (3000 mm)...'),
            (0.0, self.angular_speed, self.t_turn, '2. Belok 90 Derajat (Sudut 1)...'),
            (self.linear_speed, 0.0, self.t_short, '3. Maju Sisi Lebar (1500 mm)...'),
            (0.0, self.angular_speed, self.t_turn, '4. Belok 90 Derajat (Sudut 2)...'),
            (self.linear_speed, 0.0, self.t_long, '5. Maju Sisi Panjang (3000 mm)...'),
            (0.0, self.angular_speed, self.t_turn, '6. Belok 90 Derajat (Sudut 3)...'),
            (self.linear_speed, 0.0, self.t_short, '7. Maju Sisi Lebar (1500 mm)...'),
            (0.0, self.angular_speed, self.t_turn, '8. Belok 90 Derajat (Sudut 4)...'),
        ]

        self.current_step = 0
        self.timer = self.create_timer(0.1, self.timer_callback)
        self.step_start_time = self.get_clock().now().nanoseconds / 1e9

    def timer_callback(self):
        current_time = self.get_clock().now().nanoseconds / 1e9
        elapsed_time = current_time - self.step_start_time

        if self.current_step < len(self.steps):
            linear_x, angular_z, duration, log_msg = self.steps[self.current_step]
            
            if elapsed_time < duration:
                msg = Twist()
                msg.linear.x = linear_x
                msg.angular.z = angular_z
                self.publisher_.publish(msg)
                self.get_logger().info(log_msg)
            else:
                self.current_step += 1
                self.step_start_time = current_time
        else:
            msg = Twist()
            msg.linear.x = 0.0
            msg.angular.z = 0.0
            self.publisher_.publish(msg)
            self.get_logger().info('Selesai membentuk lintasan Persegi Panjang 3000x1500 mm. Robot Berhenti.')
            
            self.timer.cancel()
            rclpy.shutdown()

def main(args=None):
    rclpy.init(args=args)
    node = RectangleMoverNode()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass
    finally:
        if rclpy.ok():
            node.destroy_node()
            rclpy.shutdown()

if __name__ == '__main__':
    main()
