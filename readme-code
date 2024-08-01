import React, { useState, useEffect } from 'react';
import { X, ChevronUp, ChevronDown } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';

const getRandomColor = () => {
  const colors = ['#FFB3BA', '#BAFFC9', '#BAE1FF', '#FFFFBA', '#FFD9BA', '#E0BBFF'];
  return colors[Math.floor(Math.random() * colors.length)];
};

const MindFloat = () => {
  const [thoughts, setThoughts] = useState([]);
  const [newThought, setNewThought] = useState('');

  const addThought = () => {
    if (newThought.trim() !== '') {
      const thought = {
        id: Date.now(),
        text: newThought,
        x: Math.random() * (window.innerWidth - 200),
        y: Math.random() * (window.innerHeight - 100),
        dx: (Math.random() - 0.5) * 2,
        dy: (Math.random() - 0.5) * 2,
        color: getRandomColor(),
        size: 1,
      };
      setThoughts([...thoughts, thought]);
      setNewThought('');
    }
  };

  const removeThought = (id) => {
    setThoughts(thoughts.filter(thought => thought.id !== id));
  };

  const adjustThoughtSize = (id, increment) => {
    setThoughts(thoughts.map(thought => {
      if (thought.id === id) {
        const newSize = Math.max(0.5, Math.min(2, thought.size + increment));
        return { ...thought, size: newSize };
      }
      return thought;
    }));
  };

  useEffect(() => {
    const moveThoughts = () => {
      setThoughts(prevThoughts =>
        prevThoughts.map(thought => {
          let newX = thought.x + thought.dx;
          let newY = thought.y + thought.dy;

          if (newX <= 0 || newX >= window.innerWidth - 200) thought.dx *= -1;
          if (newY <= 0 || newY >= window.innerHeight - 100) thought.dy *= -1;

          return {
            ...thought,
            x: newX,
            y: newY,
          };
        })
      );
    };

    const intervalId = setInterval(moveThoughts, 50);
    return () => clearInterval(intervalId);
  }, []);

  return (
    <div className="h-screen w-full overflow-hidden relative bg-gray-100">
      <div className="p-4 bg-white shadow-md">
        <h1 className="text-2xl font-bold mb-4">MindFloat</h1>
        <div className="flex">
          <Input
            type="text"
            value={newThought}
            onChange={(e) => setNewThought(e.target.value)}
            placeholder="Enter a thought..."
            className="flex-grow mr-2"
          />
          <Button onClick={addThought}>Add Thought</Button>
        </div>
      </div>
      {thoughts.map(thought => (
        <div
          key={thought.id}
          className="absolute p-2 rounded shadow-md transition-all duration-300 ease-in-out"
          style={{
            left: thought.x + 'px',
            top: thought.y + 'px',
            maxWidth: `${200 * thought.size}px`,
            backgroundColor: thought.color,
            transform: `scale(${thought.size})`,
            zIndex: Math.floor(thought.size * 10),
          }}
        >
          <button
            onClick={() => removeThought(thought.id)}
            className="absolute top-0 right-0 p-1"
          >
            <X size={16} />
          </button>
          <div className="absolute top-0 left-0 flex flex-col">
            <button onClick={() => adjustThoughtSize(thought.id, 0.1)} className="p-1">
              <ChevronUp size={16} />
            </button>
            <button onClick={() => adjustThoughtSize(thought.id, -0.1)} className="p-1">
              <ChevronDown size={16} />
            </button>
          </div>
          {thought.text}
        </div>
      ))}
    </div>
  );
};

export default MindFloat;
