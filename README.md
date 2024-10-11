# scary-movie-ranker
User ranks inputs scary movies and ranks them from 1-10.
import tkinter as tk
from tkinter import messagebox
# Main application class
class MovieRankerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Scary Movie Ranker")

        # Set the window's background color to purple
        self.root.configure(bg="purple")

        # Main Window elements
        self.main_frame = tk.Frame(self.root, bg="purple")
        self.main_frame.pack(padx=10, pady=10)

        self.label_title = tk.Label(self.main_frame, text="Welcome to the Scary Movie Ranker!", font=("Blackadder ITC", 30), bg="purple", fg="white")
        self.label_title.grid(row=0, column=0, columnspan=2, pady=(0, 10))

        # Load images here
        self.load_images()

        # Display the first image in the main window
        self.image_label1 = tk.Label(self.main_frame, image=self.image1, bg="purple")
        self.image_label1.grid(row=1, column=0, columnspan=2, pady=(0, 10))

        self.label_movie = tk.Label(self.main_frame, text="Enter Scary Movie Name:", font=("Times New Roman", 12), bg="purple", fg="white")
        self.label_movie.grid(row=2, column=0, sticky="e", padx=(5, 2))

        self.entry_movie = tk.Entry(self.main_frame, width=30)
        self.entry_movie.grid(row=2, column=1, pady=5, sticky="w")

        self.label_rank = tk.Label(self.main_frame, text="Rank (1-10):", font=("Times New Roman", 12), bg="purple", fg="white")
        self.label_rank.grid(row=3, column=0, sticky="e", padx=(5, 2))

        self.entry_rank = tk.Entry(self.main_frame, width=5)
        self.entry_rank.grid(row=3, column=1, pady=5, sticky="w")

        # Set up buttons and set background color to yellow and text color to black
        self.add_button = tk.Button(self.main_frame, text="Add Movie", command=self.add_movie, bg="yellow", fg="black")
        self.add_button.grid(row=4, column=0, columnspan=2, pady=5)

        self.show_rank_button = tk.Button(self.main_frame, text="Show Rankings", command=self.show_rankings, bg="yellow", fg="black")
        self.show_rank_button.grid(row=5, column=0, columnspan=2, pady=5)

        self.exit_button = tk.Button(self.main_frame, text="Exit if you dare..", command=self.root.destroy, bg="yellow", fg="black")
        self.exit_button.grid(row=6, column=0, columnspan=2, pady=5)

        # Store movie rankings in a dictionary
        self.movie_rankings = {}

    def load_images(self):
        # Load the two .png images using relative paths
        self.image1 = tk.PhotoImage(file="scarymoviecharacters.png")
        self.image2 = tk.PhotoImage(file="chuckypennywiseandsaw.png")

    def add_movie(self):
        movie = self.entry_movie.get().strip()
        try:
            rank = float(self.entry_rank.get().strip())
            if not (1 <= rank <= 10):
                raise ValueError("Rank must be between 1 and 10")
        except ValueError:
            messagebox.showerror("Invalid input", "Please enter a valid rank between 1 and 10.")
            return

        if movie:
            self.movie_rankings[movie] = rank
            messagebox.showinfo("Success", f"Movie '{movie}' added with rank {rank}")
            self.entry_movie.delete(0, tk.END)
            self.entry_rank.delete(0, tk.END)
        else:
            messagebox.showerror("Error", "Please enter a movie name")

    def show_rankings(self):
        ranking_window = tk.Toplevel(self.root)
        ranking_window.title("Movie Rankings")
        ranking_window.configure(bg="purple")

        label = tk.Label(ranking_window, text="Your Scary Movie Rankings", font=("Blackadder ITC", 30), bg="purple", fg="white")
        label.pack(pady=5)

        # Display the second image in the rankings window
        image_label2 = tk.Label(ranking_window, image=self.image2, bg="purple")
        image_label2.pack(pady=10)

        sorted_rankings = sorted(self.movie_rankings.items(), key=lambda x: x[1], reverse=True)
        rankings_text = "\n".join(f"{movie}: {rank}" for movie, rank in sorted_rankings)

        ranking_label = tk.Label(ranking_window, text=rankings_text, justify="left", bg="purple", fg="white")
        ranking_label.pack(padx=10, pady=10)

# Main application execution
if __name__ == "__main__":
    root = tk.Tk()
    app = MovieRankerApp(root)
    root.mainloop()


