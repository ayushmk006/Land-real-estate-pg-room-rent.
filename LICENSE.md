import { useState } from "react";
import { Phone, MessageCircle, MapPin } from "lucide-react";

function Card({ children, className }) {
  return (
    <div className={`shadow-lg rounded-2xl overflow-hidden border ${className}`}>
      {children}
    </div>
  );
}

function CardContent({ children, className }) {
  return <div className={`p-4 ${className}`}>{children}</div>;
}

function Button({ children, className, asChild, ...props }) {
  if (asChild) {
    return (
      <span className={`px-4 py-2 rounded-lg ${className}`} {...props}>
        {children}
      </span>
    );
  }
  return (
    <button className={`px-4 py-2 rounded-lg ${className}`} {...props}>
      {children}
    </button>
  );
}

function Input({ ...props }) {
  return (
    <input
      className="border rounded-lg p-2 w-full focus:outline-none focus:ring-2 focus:ring-green-500"
      {...props}
    />
  );
}

export default function LocalRoomsWebsite() {
  const [location, setLocation] = useState("");
  const [category, setCategory] = useState("");
  const [budget, setBudget] = useState("");

  const listings = [
    {
      id: 1,
      title: "1 BHK Flat in Kankarbagh",
      price: "₹8,000/month",
      furnished: "Semi-Furnished",
      category: "Flat",
      location: "Patna, Bihar",
      image: "https://via.placeholder.com/400x250",
      contact: "6207789956",
    },
    {
      id: 2,
      title: "Room for Students in Rajendra Nagar",
      price: "₹4,500/month",
      furnished: "Furnished",
      category: "Room",
      location: "Patna, Bihar",
      image: "https://via.placeholder.com/400x250",
      contact: "6207789956",
    },
  ];

  const filteredListings = listings.filter((item) => {
    return (
      (!location ||
        item.location.toLowerCase().includes(location.toLowerCase())) &&
      (!category || item.category === category) &&
      (!budget ||
        parseInt(item.price.replace(/[^0-9]/g, "")) <= parseInt(budget))
    );
  });

  return (
    <div className="min-h-screen bg-white">
      {/* Header */}
      <header className="bg-green-600 text-white py-6 shadow-md">
        <div className="text-center">
          <h1 className="text-4xl font-bold">LocalRooms</h1>
          <p className="text-lg mt-2">
            Find & Rent Rooms, Flats, and Shops in Your Local Area
          </p>
        </div>
      </header>

      {/* Search Filters */}
      <section className="max-w-6xl mx-auto p-6">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <Input
            placeholder="Enter Location"
            value={location}
            onChange={(e) => setLocation(e.target.value)}
          />
          <select
            className="border rounded-lg p-2"
            value={category}
            onChange={(e) => setCategory(e.target.value)}
          >
            <option value="">Select Category</option>
            <option value="Room">Room</option>
            <option value="Flat">Flat</option>
            <option value="Shop">Shop</option>
            <option value="PG">PG</option>
          </select>
          <Input
            placeholder="Budget (Max Rent)"
            value={budget}
            onChange={(e) => setBudget(e.target.value)}
          />
        </div>
      </section>

      {/* Listings */}
      <section className="max-w-6xl mx-auto p-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {filteredListings.length > 0 ? (
          filteredListings.map((item) => (
            <Card key={item.id}>
              <img
                src={item.image}
                alt={item.title}
                className="w-full h-48 object-cover"
              />
              <CardContent>
                <h2 className="text-xl font-semibold">{item.title}</h2>
                <p className="text-green-600 font-bold">{item.price}</p>
                <p className="text-gray-500">
                  {item.furnished} • {item.category}
                </p>
                <p className="flex items-center text-gray-600 mt-1">
                  <MapPin className="h-4 w-4 mr-1" /> {item.location}
                </p>
                <div className="flex gap-3 mt-4">
                  <Button className="bg-green-500 hover:bg-green-600 text-white flex items-center gap-2" asChild>
                    <a href={`tel:${item.contact}`}>
                      <Phone className="h-4 w-4" /> Call
                    </a>
                  </Button>
                  <Button className="bg-green-700 hover:bg-green-800 text-white flex items-center gap-2" asChild>
                    <a href={`https://wa.me/91${item.contact}`} target="_blank">
                      <MessageCircle className="h-4 w-4" /> WhatsApp
                    </a>
                  </Button>
                </div>
              </CardContent>
            </Card>
          ))
        ) : (
          <p className="col-span-3 text-center text-gray-500">
            No listings found matching your filters.
          </p>
        )}
      </section>

      {/* Footer */}
      <footer className="bg-gray-100 mt-12 text-center py-6 border-t">
        <p className="text-gray-700">
          📞 Contact: +91 6207789956 | 💬 WhatsApp: +91 6207789956
        </p>
        <p className="text-sm text-gray-500 mt-2">
          © 2025 LocalRooms. All rights reserved.
        </p>
      </footer>
    </div>
  );
}
